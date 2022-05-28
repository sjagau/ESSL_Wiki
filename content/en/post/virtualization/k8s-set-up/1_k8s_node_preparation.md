---
title: "Kubernetes Node Preparation"
date: 2018-12-29T11:02:05+06:00
lastmod: 2020-01-05T10:42:26+06:00
icon: ti-panel
weight: 50
draft: false
description: "Preparations to go through prior to deploying a Kubernetes cluster"
type: docs
---
export KUBECONFIG=$HOME/.kube/config
on all nodes: apt-get install nfs-common cifs-utils -y
install docker and kubeadm from official documentation