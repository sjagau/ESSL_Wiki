---
title: "Deploy Kubernetes on VMs"
date: 2018-12-29T11:02:05+06:00
lastmod: 2020-01-05T10:42:26+06:00
icon: ti-panel
weight: 50
draft: false
description: "Deploy Kubernetes on VM machines running Ubuntu"
type: docs
---
In the ESSL case Oracle vBox has been used for creating and running the Ubuntu VM nodes.

## vBox Node Configurations
### 'K8s Master' Node Data

MAC-Adr: 080027AA219D

UID: augie
PWD: K8sAdm
IP-Adr: 164.48.201.220/23  --   192.168.202.3/24
Port: enp0s8

Local: 10.0.2.15/24
Port: enp0s3

Full Name: Augie Jagau
Server Name: k8s-master

Docker: 19.03 stable164.

### 'Worker-1' Node Data

UID: augie
PWD: K8sAdm
IP-Adr: 164.48.201.222/23  --   192.168.202.04/24
Port: enp0s8
MAC-Adr: 08:00:27:9D:1E:2E

Local: 10.0.2.15/24
Port: enp0s3

Full Name: Augie Jagau
Server Name: k8s-master

Docker: 19.03 stable

### 'Worker-2' Node Data

UID: augie
PWD: K8sAdm
Port: enp0s8
IP-Adr: 164.48.201.223/23  --   192.168.202.05/24
MAC-Adr: 08:00:27:69:A6:AA

Local: 10.0.2.15/24
Port: enp0s3

Full Name: Augie Jagau
Server Name: k8s-master

Docker: 19.03 stable

### 'NFS-Server' Node Data

UID: augie
PWD: K8sAdm
Port: enp0s8
IP-Adr: 164.48.201.226/23  --   192.168.202.06/24
MAC-Adr: 08:00:27:43:32:75

Local: 10.0.2.15/24
Port: enp0s3

Full Name: Augie Jagau
Server Name: k8s-master

Docker: 19.03 stable

#### Misc. Configuration Notes
`/etc/resolv.conf`:
MASTER/WORKER-1/WORKER-2
nameserver 127.0.0.53
options edns0
search de.eu.ericsson.se

ESSL
/usr/lib/systemd
nameserver 127.0.0.53
options edns

/etc/resolv.conf
search ss2k.uci.edu
nameserver 128.200.18.222
nameserver 128.195.133.145

#### vBox Network Configuration

All VMs in 'bridged' mode with '/etc/netplan/00-installer-config.yaml' configuration as follows:
network:
  version: 2
  ethernets:
     enp0s3:
         dhcp4: true
     enp0s8:
         addresses: [ip-adr/23]
         gateway4: 164.48.200.1
         nameservers:
             addresses: [127.0.0.53]

enp0s3 enp0s8 lo





KUBERNETES SUBJECT FOR STUDY
persistent Volumes -- https://kubernetes.io/docs/concepts/storage/persistent-volumes/
Secrets -- https://kubernetes.io/docs/concepts/configuration/secret/
Configure a Security Context... -- https://kubernetes.io/docs/tasks/configure-pod-container/security-context/


--------------------------------------------------------------------------------------------------------
MICRO-K8S DEVELOPMENT ENVIRONMENT
--------------------------------------------------------------
Generic Ubuntu enhanced with:
hostname: mcK8s-sandbox
installed: nfs-common cifs-utils
enabled: sudo systemctl status snap.microk8s.daemon-containerd.service
K8s-Docker secret

=========================================================================
I0109 16:12:09.662962  288586 loader.go:379] Config loaded from file:  /etc/kubernetes/admin.conf
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
I0109 16:13:46.165337  288586 round_trippers.go:445] GET https://164.48.201.220:6443/healthz?timeout=10s  in 0 milliseconds


https://stackoverflow.com/questions/57648829/how-to-fix-timeout-at-waiting-for-the-kubelet-to-boot-up-the-control-plane-as-st
https://github.com/kubernetes/kubernetes/issues/56148


https://paperhive.org/help/markdown
https://bit.dev/?utm_medium=content&utm_source=bitsandpieces&utm_content=9&utm_campaign=aug19
https://etoews.github.io/blog/2017/07/29/inject-an-executable-script-into-a-container-in-kubernetes/



-==========================================================
============================================================
Generic Ubuntu
-------------------------------
Name: August-Wilhelm Jagau
Server: generic-ubuntu
User: augie
PassWd: K8sAdm

OS: Ubuntu 20.04  Bionic

Network:
enp0s3 -- 10.02.15/24
enp0s8 -- 164.48.201.230/23  --  164.48.201.28
"/etc/netplan" w/ VB-bridging

Installed: open-ssh
Uploaded: public rsa-key

-==========================================================
TO BE CREATED
============================================================
Xtesting Machine
--------------------------
Machine: xTesting
User: augie
PWD: Renet435!

OS: Ubuntu 20.04  Bionic

Tools Installed:
PYTHON3, PIP
PyCharm
Django 3.0.6 along with asgiref-3.2.7 and sqlparse-0.3.1
django-admin-common
git, curl, Docker

Shared Folders:
755 /home/augie/Downloads/  -- Xtesting/Exchange

-----------------------------------------------------
-------------------------------------------------------
kubeadm token create --print-join-command

==============================================

NFS-Server
=================
groupadd otrlead
mkdir /var/sharedfolder
mkdir /var/sharedfolder/usrtmp
useradd -m -g otrlead -d /var/sharedfolder/usrtmp -s /usr/sbin/nologin usrtmp

create prototypical user 'usrtmp'
sudo chown -R usrtmp ./usrtmp
sudo chgrp -R otrlead ./usrtmp


==========================================

PostgeSQL configuration: /etc/postgresql/13/main/pg_hba.conf \\ postgresql.conf
sudo systemctl daemon-reload
sudo service postgresql status
sudo lsof -i -P -n | grep LISTEN

================================================

https://github.com/ubuntu/microk8s/issues/1511

modify '/var/snap/microk8s/current/args/cni-network/cni.yaml' manifest on lines 621-622:
- name: IP_AUTODETECTION_METHOD
  value: "first-found"


1: lo: inet 127.0.0.1/8
2: enp0s3: inet 10.0.2.15/24 brd 10.0.2.255
3: enp0s8: inet 164.48.201.230/23 brd 164.48.201.255


================================================
MicroK8s DNS Issue
   -- URL: https://stackoverflow.com/questions/62664701/resolving-external-domains-from-within-pods-does-not-work
-----------------------------
DNS server: 127.0.0.53

augie@generic-ubuntu$  kubectl exec -i -t dnsutils -- nslookup kubernetes.default
Server:         10.152.183.10
Address:        10.152.183.10#53

Name:   kubernetes.default.svc.cluster.local
Address: 10.152.183.1


kubectl config set-context --current --namespace=hermione-otree
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=/root/docker/config.json \
    --type=kubernetes.io/dockerconfigjson

DNS Name Format:
my-svc.my-namespace.svc.cluster.local
A record: pod-ip-address.my-namespace.pod.cluster.local




security concept
config maps
move init-container.sh
automate secret / context config

top-level script implementation


https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

Research Topics:
docker, docker-engine, docker.io, containerd, runc
https://kubernetes.io/docs/setup/cri/
journalctl --vacuum-...


environment variables
/var/lib/kubelet/confi.yaml
sysctl -system
swapoff -a

https://web.yammer.com/main/threads/eyJfdHlwZSI6IlRocmVhZCIsImlkTioiMTIwNTFvMDA1ODq2NzcxMiJ9


