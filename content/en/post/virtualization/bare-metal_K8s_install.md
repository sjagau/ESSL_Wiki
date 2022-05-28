---
title: "Kubernetes Installation on Bare-Metal"
description: "... describes the installation of Kubernetes directly on Dell-HW running Ubuntu"
date: 2021-09-06T12:12:30+02:00
to-do-note: "based on Word document: Part 2 - Kubernetes Installation on Bare-Metal"
type: docs
draft: true
weight: 40
tags: ["generic", "pending completion"]
---
The following is a step-by-step guide to install Kubernetes on a cluster of machines running _Ubuntu XX.YY_ using the Linux 4.19.0 kernel.

1. Check Availability of Container Runtime
<br>
Make sure docker (latest version) is installed.
<br>
If not, follow the instruction on Install Docker Engine on Ubuntu

2.	Verify Host Names
<br>
Check `/etc/hosts` for this content in addition to the local host entry:
```128.200.135.194	c16080100	c16080100.ss2k.uci.edu
128.200.135.204	c16080102	c16080102.ss2k.uci.edu
128.200.135.203	c11100502	c11100502.ss2k.uci.edu
128.200.135.220	c13031900	c13031900.ss2k.uci.edu
```

3.	Check Open Ports
<br>
Kubernetes ports (`179/tcp`, `4789/udp`, `5473/tcp`, `443/tcp`, `6443/tcp`, `2379/tcp`, `4149/tcp`, `10250/tcp`, `10255/tcp`, `10256/tcp`, `9099/tcp`, `6443/tcp`) are not used, i.e. they are open.
<br>
This can be checked by running:
<br>
`sudo netstat -tulpn`

4.	Disable Swapping
<br>
Disable swapping by running:
`sudo swappoff -a`
<br>
To disable the swapping permanently, edit the `/etc/fstab` file and comment-out the line in bold-face:
```
UUID=6880a28d-a9dc-4bfb-ba47-0876b50e96b3 /         ext4    errors=remount-ro 0       1
UUID=7350e6f2-e3a7-4d80-9a95-8741c7db118f /home     ext4    defaults          0       2
UUID=E2E26AD1E26AAA0D /media/windows  ntfs    defaults,umask=007,gid=46       0       0
```
Swap a usb extern (3.7 GB):
<br>
`#/dev/sdb1 none swap sw 0 0`

5.	Turn Off NetworkManager
<br>
Disable network-manager by running:
<br>
`sudo systemctl disable network-manager`
<br>
and reboot the machine!

6.	Turn Off UFW
<br>
Disable UFW by running:
<br>
`service ufw stop`

7.	Pre-Configure IP-Bridging
<br>
Check if installed:
<br>
`sudo modprobe br_netfilter`
<br>
Check on the current Kubernetes relevant IP-bridging configuration by viewing the contents of the file called `/etc/sysctl.d/kubernetes.conf`
<br>
If the file does not exist, create it with the following contents:
```
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
```
To [re-]activate the new configuration run sudo sysctl --system.

8.	Reset System for Master Installation
<br>
In case it becomes necessary to uninstall and purge Kubernetes completely from a _Ubuntu_ Linux node follow these instructions:
<br>
```
kubeadm reset --force
sudo rm -rf /etc/cni /opt/cni /var/lib/cni /etc/kubernetes /var/lib/dockershim /var/lib/etcd /var/lib/kubelet /var/run/kubernetes
(the following iptables commands need to be executed as root)
iptables -F && iptables -X
iptables -t nat -F && iptables -t nat -X
iptables -t raw -F && iptables -t raw -X
iptables -t mangle -F && iptables -t mangle -X
(exit root mode)
sudo systemctl restart docker
```

9.	IP Packet Forwarding
<br>
Make sure br-netfilter  is loaded:
<br>
`sudo modprobe br_netfilter`
<br>
Ensure the proper bridging of IP packets by configuring the iptables:
<br>
```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```

10.	Initialize Master Node
<br>
To deploy the Kubernetes master node using the kubeadm run the following command:
<br>
`sudo kubeadm –-config=/path-to-file/Kubeadm_min-config.yaml
sudo kubeadm --apiserver-advertise-address=128.200.135.194 --pod-network-cidr=192.168.0.0/16 --service-cidr=10.98.0.0/16`

11.	Initialize Calico Networking
<br>
`CALICO_IPV4POOL_CIDR`
<br>
To install Calico v3.17.0 use kubectl and run:
<br>
`kubectl apply -f Calico-3.16.5_OnPremise.yaml`
<br>
Apply the following changes:
<br>
•	line **3710**: enter value `192.168.0.0/16` to match data in Kubeadm manifest file
<br>
•	line **3678**: enter NIC port value string ```“interface=enp0*, eth*”```

12.	To Add Worker Nodes to Cluster
<br>
For adding worker nodes, start by following the instructions in step **1.** through **8.**
<br>
Then use command output on the master node when it was initialized.
<br>
Alternatively run:
<br>
`sudo kubeadm token create --print-join-command`
<br><br>
**Note**:
<br>
Depending on what has been done prior, it may not hurt to execute
<br>
`sudo kubeadm reset`
