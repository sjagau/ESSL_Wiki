---
title: Connect ESSL-Cluster Pods to ESSL-DataServer
author: "Stephan Jagau"
description: Outline of what is required to use the _ESSL-DataServer_ for persisting files using the NFS-service.
summary: “Describes how to add persistent, NFS based file storage to the Kubernetes pods running in the ESSL-Cluster.”
type: docs
weight: 10
to-do-note: "none"
date: "2021-05-13"
draft: true
tags: ["configuration", "persisting-data"]
---

## NFS Server Configuration
On *ALL* nodes interworked in the ESSL-Cluster run and install `sudo apt-get install nfs-common cifs-utils -y`. On the actual NFS-Server install also '_nfs-core-server_' by running the `apt-get` command.

### Configure Shared Volume for oTree Operations
* `groupadd otrlead`
* `mkdir /var/sharedfolder`
* `mkdir /var/sharedfolder/usrtmp`
* `useradd -m -g otrlead -d /var/sharedfolder/usrtmp -s /usr/sbin/nologin usrtmp`

### Configure usrtmp Prototype
* `create prototypical user 'usrtmp'`
* `sudo chown -R usrtmp ./usrtmp`
* `sudo chgrp -R otrlead ./usrtmp`

## Creation of File Volumes for Persisting Data
Kubernetes supports three different methods for the creation of files to persist data across the life-time of a pod instance:
* Direct NFS-Volume Attachments
* PersistenVolumes / PersistentVolumeClaims
* Automated File Attachment using NFS-Provisioner

### Direct NFS-Volume Attachmnets
copy specs from oTree pod
### PersistentVolumes / PersistentVolumeClaims
text
### NFS-Provisioner Managed Volumes
text