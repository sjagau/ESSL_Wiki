---
title: "Kubenetes Cluster Start-Up"
date: 2018-12-29T11:02:05+06:00
lastmod: 2020-01-05T10:42:26+06:00
icon: ti-panel
weight: 50
draft: false
description: "Configuration steps needed to bring up a Kubernetes cluster."
type: docs
---
The configuration steps described here not only cover the start-up of the master and worker nodes in the cluster, but also address the steps required to provide a clean, K8s-ready OS environment.

Use: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
apt-get install <package>=<version>
Installed: 1.19.6-00;  1.19.7-00
Latest: 1.20.2.-00


#### Available Plug-In's:
prometheus
microk8s
etcd

#### Node Re-Start and Clean-Up
sudo swapoff --all

For cleaning up VMs
<br>
`sudo rm /var/log/*.0`
<br>
`sudo rm /var/log/*.1`
sudo truncate -s 0 /var/log/wtmp
sudo truncate -s 0 /var/log/syslog
sudo truncate -s 0 /var/log/kern.log
sudo truncate -s 0 /var/log/dpkg.log
sudo truncate -s 0 /var/log/auth.log
sudo truncate -s 0 /var/log/cloud-init.log
sudo truncate -s 0 /var/log/cloud-init-output.log
sudo truncate -s 0 /var/log/wtmp
sudo truncate -s 0 /var/log/dmesg
sudo truncate -s 0 /var/log/installer/curtin-install.log
sudo truncate -s 0 /var/log/installer/installer-journal.txt
sudo truncate -s 0 /var/log/installer/subiquity-debug.log.1840`


RESET CLUSTER COMMANDS

sudo kubeadm reset -f
sudo docker rmi <images>
sudo systemctl stop/start docker
sudo rm -rf /etc/cni /opt/cni /var/lib/cni /etc/kubernetes /var/lib/dockershim /var/lib/etcd /var/lib/kubelet /var/run/kubernetes ~/.kube/*
sudo kubeadm init [--v=1|2|3|4|5] --apiserver-advertise-address=164.48.201.220 --pod-network-cidr=192.168.0.0/16 --service-cidr=10.98.0.0/16
kubectl apply -f calico.yaml

sudo kubeadm join 164.48.201.220:6443 --token pbl6m1.i5zwnr5h6m2s39oq \
    --discovery-token-ca-cert-hash sha256:ba7dd96fc4df75d3d7e747c14376420f4c3c0074f2de8cc073cfbb9b4e573272


kubectl create secret docker-registry new-nfs --docker-server=hub.docker.com --docker-username=codebird --docker-password=JessApf435$ --docker-email=au-wi.jagau@ieee.org

Make sure you are logged in when performing this command:
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=/root/docker/config.json \
    --type=kubernetes.io/dockerconfigjson

kubectl port-forward --address 0.0.0.0 svc/otree-deploy 9000:8000