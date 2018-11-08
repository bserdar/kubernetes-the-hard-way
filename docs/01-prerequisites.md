# Prerequisites

## Hosts

This version of the tutorial installs Kubernetes on bare-metal hosts
or VMs on a home network. This has some differences from the original
Kelsey Hightower tutorial, notably:

 - This is for bare-metal servers or VMs on trusted network
 - All VMs are on the same network. This means, if you have
   multiple hosts running vms, you have to setup networking
   so that the VMs can talk to each other. In my case, I have
   a Linux bridge setup for my server so my VMs show up in the
   same network as the rest of my home network (192.168.1.0/24).
 - Host name resolution is done with /etc/resolv.conf
 - Networking is done using kube-router
 - OS image is Centos 7
 - There is one controller and three worker nodes. Later I may try
   HA setup.


This is what I have:

```
192.168.1.174 controller-1
192.168.1.146 worker-1
192.168.1.181 worker-2
192.168.1.115 worker-3
```

Stop and disable firewalld on all hosts. This causes problems down the road.

```
systemctl stop firewalld
systemctl disable firewalld
```

Disable swap on all hosts:

```
swapoff -a
```

Comment out swap in /etc/ftab

Update /etc/hosts on each host and localhost to add your host list.

Set the hostname on each host correctly:


```
echo <hostname> /etc/hostname
hostname -b <hostname>
```

This tutorial runs with root user on all nodes.

A note before moving forward, because this confused me in the past:

The address 10.200.0.0/16, which will be used as pod CIDR, is the
address space allocated for pods. Each created pod will receive an IP
in this subnet.

The address 10.32.0.0/16 will be used as the service address
space. When you expose services, this is the address range they will
be allocated from.

In this installation, I'm using kube-router, and it sets up these
ranges nicely.

Next: [Installing Client Tools](02-client-tools.md)
