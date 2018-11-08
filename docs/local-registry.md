# Deploying a docker registry for development

I need a docker registry to push my development images, and have the
Kubernetes cluster pull from it. The proper way of doing this is to
deploy a registry image on the cluster, but that'll need storage,
which I don't have yet. So I'll deploy a registry outside the
kubernetes cluster for now.

 - Create a new VM, lregistry
 - ssh root@lregistry
 - Set hostname
```
echo lregistry /etc/hostname
hostname -b lregistry
```

 - Create self-signed certs

```
mkdir certs
openssl req   -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key   -x509 -days 365 -out certs/domain.crt
Country Name (2 letter code) [XX]:US
State or Province Name (full name) []:Colorado
Locality Name (eg, city) [Default City]:Denver
Organization Name (eg, company) [Default Company Ltd]:Home
Organizational Unit Name (eg, section) []:Basement
Common Name (eg, your name or your server's hostname) []:lregistry
Email Address []:theemail
```
This gives:

```
certs/domain.crt
cert/domain.key
```

Registry config /root/config.yml:
```
version: 0.1
log:
  fields:
    service: registry
storage:
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
http:
  addr: :443
  headers:
    X-Content-Type-Options: [nosniff]
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
```



Start registry:
```
docker run -d -p 443:443 --restart=always --name registry \
   -v /var/lib/registry:/var/lib/registry \
   -v /root/config.yml:/etc/docker/registry/config.yml \
   -v /root/certs:/certs \
   -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
   -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
   registry:2
```

At this point, any registry request will give:

```
Error response from daemon: Get https://lregistry:443/v2/: x509: certificate signed by unknown authority
```

Logout from lregistry.

You have to trust the registry locally on all the machines you intend
to push images, plus the worker nodes.


Running this on my laptop:
```
scp root@lregistry:/root/certs/domain.crt .
cp domain.crt /etc/pki/ca-trust/source/anchors/lregistry.crt 
update-ca-trust
systemctl restart docker
```

Send the cert to worker nodes:

```
for f in worker-{1,2,3}; do
  scp /etc/pki/ca-trust/source/anchors/lregistry.crt root@$f:/etc/pki/ca-trust/source/anchors
done
```

On each worker:
```
update-ca-trust
systemctl restart containerd
```

Now nodes should be able to pull from lregistry.

To push images:

```
  docker tag <myimage> lregistry:443/<myproject>/<myimage>
  docker push lregistry:443/<myproject>/<myimage>
```

And use the images with:

```
lregistry:443/<myproject>/<myimage>
```

The port appears to be necessary because of Docker's registry/image
name conventions.
