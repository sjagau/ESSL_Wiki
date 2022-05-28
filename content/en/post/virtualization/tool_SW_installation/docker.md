---
title: "Docker Deployment"
author: "Stephan Jagau"
description: ... describes the installation, configuration, and maintenance of a Docker runtime.
type: docs
to-do-note: "t.b.d."
date: "2021-09-07"
weight: 20
draft: true
tags: ["installation", "configuration"]
---
### Docker Installation
Follow the [Docker Installation](https://docs.docker.com/engine/install/ubuntu/) to set-up a Docker _container runtime_ on each of the Ubuntu instances running on nodes that are ear-marked to participate in the _ESSL-Cluster_.

### Docker Configuration
In addition, check the following files for Docker related configuration settings:
* `/etc/docker/daemon.json`
* `/etc/containerd/config.toml`
* `/var/lib/kubelet/config.yaml`

docker files in ‘/sys/fs/cgroup’

### Docker Management
do regular clean-ups of images and containers
