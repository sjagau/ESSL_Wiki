---
title: "Kubernetes Cluster Configuration"
description: "... describes the set-up of K8s nodes and the Calico container runtime"
date: 2021-09-09T11:00:30+02:00
weight: 10
to-do-note: "Add intr-page links"
type: docs
draft: true
tags: ["configuration", "deployment"]
---
## Kubernetes Cluster SW Management
This page documents the installation and configuration of the Kubernetes SW on the nodes of the ESSL-Cluster.

At the end of this page a section is included which describes how to remove the Kubernetes SW from an ESSL-Cluster node >_include a link_<.

### Prepare for K8s Cluster Set-Up
On all nodes, which are designated to be become part of the Kubernetes cluster, verify the following and make adjustments as needed.

#### Check Open Ports
Kubernetes ports (179/tcp, 4789/udp, 5473/tcp, 443/tcp, 6443/tcp, 2379/tcp, 4149/tcp, 10250/tcp, 10255/tcp, 10256/tcp, 9099/tcp, 6443/tcp) are not used, i.e. they are open.
This can be checked by running:
sudo netstat -tulpn

####	Disable Swapping
Disable swapping by running:
sudo swappoff -a
(edit /etc/fstab for permanently disabling swapping)

####	Configure Network Manager
The best approach is to disable the network-manager if it is installed, so run:
	sudo systemctl disable network-manager

####	UFW Firewall
Make sure the UFW firewall is not present or at least disabled by running.
	sudo ufw status

####	Pre-Configure IPTABLES
Also, create a file called /etc/sysctl.d/kubernetes.conf with the following contents:
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
To activate the new configurations run sudo sysctl --system.
To check on the current iptables configuration run iptables-save
text

####	IPTABLES Management
“Make sure the host is using iptables in legacy mode".
update-alternatives --set iptables /usr/sbin/iptables-legacy
https://github.com/kubernetes/kubernetes/issues/71305

###	Set-Up of Cluster Master Node
text.

####	Regular / Normal Procedure
text
1	have two or three Linux machines on hand (one master, at least one worker)
2	 make sure you have superUser access to the machines (uid/pwd)
3	 configure network access so that both machines are accessible across Internet
4	verify git, curl, and Docker are installed, if not add
5	set up a Docker registry or make sure Docker hub is reachable from machines in K8s cluster
6	set up a basic cluster following tutorial instructions at
either: https://www.digitalocean.com/community/tutorials/how-to-create-a-kubernetes-cluster-using-kubeadm-on-debian-9
or https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
or:   https://www.howtoforge.com/tutorial/how-to-install-kubernetes-on-ubuntu/
(may mention previous steps 1. — 4.; ignore and/or selectively complete)
7	implement and verify cluster monitoring to be set-up and working
 (Grafana - ElasticSearch - Prometheus)
8	create a basic application Helm chart to make sure cluster is working properly;
execute the Helm chart (this step may have been part of the tutorials!)
9	decide on load-balancing scheme for appropriately distributing traffic to worker nodes
10	configure Kubernetes cluster network;
make sure to use ‘service endpoints’ and not IP addresses to access applications
11	configure a storage hierarchy consisting of ‘in-memory’ storage for direct pod access
and ‘persisted’, physical (NFS) file and data-base (PostgreSQL, MySQL) storage
(user PostgreSQL for oTree/Django runtime support!!)
12	extend Kubernetes cluster as other HW worker nodes become available;
13	make sure these nodes are visible in the monitoring and participate in the load-sharing
The above should be aligned with the contents of the ‘step-by-step’ K8s installation instructions.
Do not forget to execute `export KUBECONFIG=$HOME/.kube/config`

####	Procedure for Re-Trying ‘kubeadm init’ Command on K8s Master Node
In case ‘kubeadm init’ has got to be re-run on the K8s Master node for any reason, follow this procedure in order listed:
•	kubeadm reset --force
•	sudo rm -r /etc/cni
•	sudo systemctl stop docker
•	sudo systemctl restart docker
•	sudo systemctl status docker
•	sudo systemctl restart kubelet
•	sudo systemctl status kubelet
(confirm that kubelet is not yet running)
•	sudo kubeadm init \
•	--pod-network-cidr=192.168.0.0/16 \
•	--control-plane-endpoint=k8s-cluster.computingforgeeks.com (verify semantics)
•	--ignore-preflight-errors=Swap
In the vast majority of cases this procedure will allow for ‘kubeadm init’ to be executed properly.

###	Cluster Networking
Cluster networking encompasses two areas: internal cluster networking and access to the cluster from external nodes.

####	Internal Cluster Networking Using Calico CNI
The Calico ‘container network interface’ is one of the most widely used Kubernetes plug-ins for internal pod-to-pod networking and policy control. For its installation follow the instructions below.
Note: The command ‘calicoctl node checksystem’ does not run, according to several web-pages the may be safely ignored ( https://github.com/kubernetes-sigs/kubespray/issues/6289 ; https://github.com/projectcalico/calicoctl/issues/2204 )
Follow section 6 Install CNI plugin for Kubernetes on the page How to Install Kubernetes Cluster on CentOS 8 (most of the rest of the flow has been adopted for the general K8s installation approach laid out above).
Instead of running the manifest.yaml file directly from the Internet, download the file first. Search for the ‘IP_AUTODEDECTION_METHOD’ directive and adjust the value to read “interface=enp0.*” (this will accommodate physical Ethernet port names such as enp0s31f6 or enp0s25 which are used on Dell machines, for instance). (see https://docs.projectcalico.org/reference/node/configuration#ip-autodetection-methods for more information on the subject)
Run ‘kubeadm apply -f Calico_CNI-Node_Manifest.yaml’
The output is fairly voluminous, however each line should end in ‘configured’ indicating that all components of CNI have been properly set up up.
For verification run ‘kubeadm -n kube-system get pods’. This should yield an output which resembles the following screen-shot:  
(copied from ‘Running a Kubernetes Cluster on Ubuntu with Calico’)

####	Connect from External Nodes to K8s Cluster
text

###	Set-Up of Worker Nodes
text.
In addition to following the normal process of having a node join a [master node] cluster it may be necessary to run:
kubeadm token create --print-join-command
and use the output for the join command.
Execute the above in response to error messages indicating among others ‘JWS token not being created…’.

###	Cluster Storage Hierarchy
text.

###	Cluster Monitoring
use fluentd in a separate pod

###	Complete Kubernetes Deinstallation
text

####	Procedure Proven Working for Debian Linux Bare-Metal Installations
In case it becomes necessary to uninstall and purge Kubernetes completely from a Debian Linux node follow these instructions:
kubeadm reset -f
rm -rf /etc/cni /opt/cni /var/lib/cni /etc/kubernetes /var/lib/dockershim /var/lib/etcd /var/lib/kubelet /var/run/kubernetes ~/.kube/*
(the following iptables commands need to be executed as root)
iptables -F && iptables -X
iptables -t nat -F && iptables -t nat -X
iptables -t raw -F && iptables -t raw -X
iptables -t mangle -F && iptables -t mangle -X
systemctl restart docker
(source: StackOverflow: K8s Complete Deinstallation – instructions copied from very end of page)

####	If All Else Fails
Tear up the cluster, clean up environment, and then install:
•	docker <latest stable version>,
•	kubeadm <latest stable version>,
•	kubelet <latest stable version>,
•	kubectl <latest stable version>, and
•	calico <latest stable version>
This should bring the cluster back up also.

####	The Ultimate Recovery Process
To be completely on the safe side, consider re-installing Debian Linux and then the K8s cluster software listed in sec.4.6.2 immediately above.

### Remove Kubernetes SW from an ESSL-Cluster Node
If for some reason the Kubernetes SW needs to be completely removed from one of the ESSL-Cluster nodes, follow the steps listed here:
* `sudo kubeadm reset`
* `sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*` 
* `sudo apt-get autoremove`
* `sudo rm -rf ~/.kube`

This procedue has been copied from the [How to Completely Un-Install Kubernetes](https://stackoverflow.com/questions/44698283/how-to-completely-uninstall-kubernetes) _StackOverflow_ page.

*Note*: The above is not required for updating the Kubernetes SW. Please check the [Kubernetes SW Update/Upgrade](http://localhost:1313) page for more details.