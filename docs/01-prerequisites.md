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
192.168.1.128 controller-1
192.168.1.177 worker-1
192.168.1.197 worker-2
192.168.1.48 worker-3
```

Stop and disable firewalld on all hosts. This causes problems down the road.

Update /etc/hosts on each host and localhost to add your host list.

This tutorial runs with root user on all nodes.

Next: [Installing Client Tools](02-client-tools.md)
