# Kubernetes The Hard Way - Bare Metal Edition

This tutorial is copied and modified from [Kelsey Hightower's original](https://github.com/kelseyhightower/kubernetes-the-hard-way/tree/master/docs)

It installs a Kubernetes cluster on a home network with bare-metal
nodes or VMs. At this point it sets up one controller and three worker
nodes. You can add as many workers as you want, but adding new controllers
will require a load balancer.

This uses [kube-router](https://github.com/cloudnativelabs/kube-router) to setup pod networking.

I am converting this to install a fully functional k8s cluster for
home, with an integrated registry and storage. I'll update this
tutorial as I build those pieces.

## Labs


* [Prerequisites](docs/01-prerequisites.md)
* [Installing the Client Tools](docs/02-client-tools.md)
* [Provisioning Compute Resources](docs/03-compute-resources.md)
* [Provisioning the CA and Generating TLS Certificates](docs/04-certificate-authority.md)
* [Generating Kubernetes Configuration Files for Authentication](docs/05-kubernetes-configuration-files.md)
* [Generating the Data Encryption Config and Key](docs/06-data-encryption-keys.md)
* [Bootstrapping the etcd Cluster](docs/07-bootstrapping-etcd.md)
* [Bootstrapping the Kubernetes Control Plane](docs/08-bootstrapping-kubernetes-controllers.md)
* [Bootstrapping the Kubernetes Worker Nodes](docs/09-bootstrapping-kubernetes-workers.md)
* [Configuring kubectl for Remote Access](docs/10-configuring-kubectl.md)
* [Provisioning Pod Network Routes](docs/11-pod-network-routes.md)
* [Deploying the DNS Cluster Add-on](docs/12-dns-addon.md)
* [Smoke Test](docs/13-smoke-test.md)

More stuff:

* [Deploying an internal Docker registry](docs/local-registry.md)
