---
title: "Deploy and Configure Data-Server"
date: 2018-12-29T11:02:05+06:00
lastmod: 2020-01-05T10:42:26+06:00
icon: ti-panel
weight: 50
draft: false
description: "Deploy and configure a data-server in support of the storage needs of a Kubernetes cluster"
type: docs
---
In the ESSL case the 'data server' includes an NFS server and a PostgreSQL database management system

### POSTGRESQL DATABASE SERVER
  URL: https://ibsen-otree-docs.ibsen-h2020.eu/server/ubuntu.html#database-postgres

pg_ctlcluster 13 main start
sudo su - postgres
psql

CREATE DATABASE django_db;
CREATE USER otree_user WITH PASSWORD 'mypassword';
GRANT ALL PRIVILEGES ON DATABASE django_db TO otree_user;

Deployment Specific:

DB: otree_data
UID: otree_user
PWD: otree

### NFS File-Server
`sudo apt update`
<br>
`sudo apt install nfs-kernel-server`
<br>
`sudo cat /proc/fs/nfsd/versions`
<br>
`sudo exportfs -ar`
<br>
`sudo exportfs -v`
<br>
`sudo systemctl restart nfs-kernel-server`
<br>
`sudo ufw allow from ip-address/cidr to any port nfs`
<br>
`sudo ufw staus`
<br>
Source: [Configure an NFS Server on Ubuntu 20.04](https://linuxize.com/post/how-to-install-and-configure-an-nfs-server-on-ubuntu-20-04/)