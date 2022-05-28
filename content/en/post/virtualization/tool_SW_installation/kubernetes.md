---
title: "Kubernetes Installation"
description: ... describes the installation of the three basic Kubernetes tools kubelet, kubeadm, and kubectl.
author: "Stephan Jagau"
summary: "This one of several pages describing how to set up, configure, and start running an operational Kubernetes cluster."
type: docs
to-do-note: "t.b.d."
date: "2021-09-07"
weight: 40
draft: true
tags: ["configuration", "installation"]
---
### Overview
A _Kubernetes_ cluster at the control plane level is managed with the help of three tools: `kubeadm`, `kubelet`, and `kubectl`.

The tool `kubelet` manages each individual node participating in the cluster. It exposes an API, which supports the execution of commands for managing the Kubernetes resouces and _container-runtime_ instances located the node it is serving.

`kubeadm` is used to start and stop _Kubernetes_ related processes on a node participating in the cluster. The tool supports differnt commands for starting the _Master_ and _Worker_ nodes. Moreover, there is a command to _reset_ a Kubernetes node.

For details concerning _Kubernetes_ and its functions and features consult the [Kubernetes](https://kubernetes.io/docs/home/) documentation.

### Kubernetes Node Start-Up
For an installation procedure follow the instructions on the [Install Kubernetes on Ubuntu](https://www.edureka.co/blog/install-kubernetes-on-ubuntu) page.

To deploy the Kubernetes master node using a _baseline_ configuration use the following `kubeadm` command:
```
sudo kubeadm config print init-defaults > default-config.yaml
```

Open the file `default-config.yaml` putput from the previous _kubeadm_ command and edit the three (3)lines which refer to UCI ESSL data, i.e.:
```
	localAPIEndpoint.advertiseAddress: 128.200.135.19
	nodeRegistration.name: c16080100
	clusterName: essl-cluster
```

(Do not upset the layout of the file; just change the values, if needed)

To bring up the Kubernetes master node run the following kubeadm command:
```
sudo kubeadm init -f default -config.yaml
```

### Intra-Node Networking
For communications between the various nodes making up a Kubernetes cluster a '_container network interface_' (CNI) is required. As per the [Comparing Kubernetes CNI Providers](https://rancher.com/blog/2019/2019-03-21-comparing-kubernetes-cni-providers-flannel-calico-canal-and-weave/) page currently four alternatives are available and can be chosen from:
| Name | Remarks |
|----|----|
| **Calico** | networking and security control |
| **Canal** | combines Calico and Flannel features |
| **Fannel** | straightforward, most popular plug-in |
| **WeaveNet** | mesh network for routing flexibility |

For the _ESSL-Cluster_ **Calico v3.16.5** has been chosen.

To install Calico first bring up the nodes of the cluster ([see above]()) and then run `kubectl apply -f /path/to/Calico_v3.16.5.yaml`. If Calico has been started correctly, similar entries as shown in the screenshot below should be displayed in response to the `kubectl -n kube-system get nodes` command after a minute or so: