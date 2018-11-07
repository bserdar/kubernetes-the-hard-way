# Provisioning Pod Network Routes

Pods scheduled to a node receive an IP address from the node's Pod CIDR range. At this point pods can not communicate with other pods running on different nodes due to missing network 


> There are [other ways](https://kubernetes.io/docs/concepts/cluster-administration/networking/#how-to-achieve-this) to implement the Kubernetes networking model.

## The Routing Table

We setup the node addresses as follows:

```
192.168.1.177 worker-1  pod cidr: 10.200.1.0/24
192.168.1.197 worker-2  pod cidr: 10.200.2.0/24
192.168.1.48  worker-3  pod cidr: 10.200.3.0/24
```

## Routes

Simplest solution is running kube-router without service proxy:

kubectl apply -f https://raw.githubusercontent.com/cloudnativelabs/kube-router/master/daemonset/generic-kuberouter.yaml


Next: [Deploying the DNS Cluster Add-on](12-dns-addon.md)


