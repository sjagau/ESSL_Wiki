---
title: "NFS-Provisioner Set-Up"
author: "Stephan Jagau"
description: "Configure an NFS-Provisioner in a Kubernetes cluster and connect to file volume"
type: docs
icon: ti-panel
weight: 10
to-do-note: ["Add summary to 'NFS-Provisioner Set-P' page", "see 'check' note' below"]
date: "2021-10-03"
draft: true
tags: ["configuration", "operations"]
---
Check <file reference> for how an NFS server can be set up that the NFS-Provisioner will connect to.

### NFS Service Set-Up
In order to persist files using NFS it is neccessary to have an NFS server deployed. This is done by installing the `text` and `cifs-utils` packages:
```sh
sudo apt update
sudo apt install -y nfs-kernel-server cifs-utils
```
The NFS server will automatically start running.

For ESSL-Cluster nodes to be able to use the NFS server, the `nfs-common` and 'cifs-utils` packages must be installed:
```sh
sudo apt update
sudo apt install -y nfs-common cifs-utils
```

#### NFS-Provisioner
```sh
helm repo add ckotzbauer https://ckotzbauer.github.io/helm-charts
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm -n essl-test install test-nfs-prov \
    --set nfs.server=164.48.201.226 \
    --set nfs.path=/var/autoshare ckotzbauer/nfs-client-provisioner
helm repo add nfs-provisioner
helm repo update https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner
helm -n target-namespace -nfs install nfs-prov nfs-provisioner/nfs-subdir-external-provisioner \
  --set nfs.server=nfs-server-ip \
  --set nfs.path=/path/to/shared/volume \
  ----set storageClass.defaultClass=true --generate-name
```
Follows instructions on: `https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner]`

```sh
cd /path/to/helm/chart/redis
helm -n essl-test install --timeout 600s redis-cluster --values values-production.yaml .
```

#### NFS Volumes Set-Up
Perform the following steps to create a volume `autoshare` that will be exposed by the NFS server:
```sh
sudo mkdir /opt/autoshare
sudo chown nobody:nogroup /opt/autoshare
```
"update /etc/exports"
```sh
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```
 
#### REDIS Cluster
```sh
helm -n essl-test install --timeout 600s redis-cluster bitnami/redis-cluster
```