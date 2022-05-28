---
title: "oTree Experimentor Space Make-Up"
author: "Stephan Jagau"
description: ... provides a high-level overiew of what is included in ESSL-Cluster _oTree Experimentor Work-Spaces_.
type: docs
date: "2021-11-26"
weight: 40
to-do-note: "Key elements making up an oTree Experimentor Work-Space"
draft: true
tags: ["configuration", "deployment"]
---
The design of the ESSL-Cluster '_Experimentor Work-Spaces_' closesly follows the recommendations for setting up conventional oTree work-spaces. Unlike what is described in the official **[oTree](https://www.otree.org)** documentation, the work-spaces in the ESSL-Cluster operate as containers in Kubernetes pods.

**[Kubernetes](https://kubernetes.io/docs/concepts/overview/)** is a _control plane_ system, which allows for the fully automated management of multiple containers on a given set of HW nodes. The overview page referenced here also provides links to pages that aid in the installation of _Kubernetes_.

For the ESSL-Cluster a total of currently four _Dell_ server machines is being used:
| Node Name | Cluster Role | ss2k.uci.edu | Dell Model | Installed OS |
| ---- | ---- | ---- | ---- | ---- |
| K8s-MASTER | _K8s Master_ | C16080100 | Precision Tower 3620 | Ubuntu 18.04 LTS |
| K8s-WORKER-1 | _K8s Worker_ | C11100502 | Optiplex 990 | Ubuntu 18.04 LTS |
| K8s-WORKER-2 | _K8s Worker_ | C16080102 | Optiplex 7040 | Ubuntu 18.04 LTS |
| Data Server | _DB_ & _NFS_ Svr | C13031900 | PowerEdge T310 | Ubuntu 18.04 LTS |

**Note**: Futher details concerning the configuration of the HW nodes making up the ESS-Cluster are available on the **[ESSL-Cluster Infra-Structure]( {{< ref "/cluster_docs/ESSL-cluster_inventory" >}} )** page.

For an example for how to install _Ubuntu_ on Dell machines open the **[OS Installation of Power Edge Servers](https://www.dell.com/support/kbdoc/en-us/000130160/how-to-install-the-operating-system-on-a-dell-poweredge-server-os-deployment?lwp=rt)** page. Other _Dell Support_ pages exist which describe the OS installation on the other Dell machines used in the ESSL-Cluster.

To review details about the OS installed on the _Dell_ machines open the **[Canonical Ubuntu](https://ubuntu.com/)** landing page.

The _data-base_ server used is **[PostgreSQL for Linux/Ubuntu](https://www.postgresql.org/download/linux/ubuntu/)** and the **[NFS on Ubuntu 20.04](https://linuxize.com/post/how-to-install-and-configure-an-nfs-server-on-ubuntu-20-04/)** page provides further documentation for the implemented instance of a _network file system_ (NFS).