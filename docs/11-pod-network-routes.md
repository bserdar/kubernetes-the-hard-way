# Provisioning Pod Network Routes

Pods scheduled to a node receive an IP address from the node's Pod CIDR range. At this point pods can not communicate with other pods running on different nodes due to missing network 


> There are [other ways](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-achieve-this) to implement the Kubernetes networking model.

## Routes

Simplest solution is running kube-router with service proxy:

```
wget https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/generic-kuberouter-all-features.yaml
```

Edit the generic-kuberouter-all-features.yaml file and change clusterCIDR and api server:
```
 ...
 kubeconfig: |
    apiVersion: v1
    kind: Config
    clusterCIDR: "10.200.0.0/16"
    clusters:
    - name: cluster
      cluster:
        certificate-authority: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
        server: https://192.168.1.174:6443
```

```
kubectl apply -f generic-kuberouter-all-features.yaml
```

This should run kube-router on all worker nodes. After this:

```
kubectl get nodes
```

should print:

```
NAME       STATUS   ROLES    AGE     VERSION
worker-1   Ready    <none>   35m     v1.12.0
worker-2   Ready    <none>   5m9s    v1.12.0
worker-3   Ready    <none>   2m28s   v1.12.0
```

If for some reason you need to reinstall kube-router or change the
config, check the /var/lib/kube-router/kubeconfig file. That's
initialized when kube-router is installed, and stays there even if you
remove it, so if you change addresses, make sure that file is up to
date.

Next: [Deploying the DNS Cluster Add-on](12-dns-addon.md)


