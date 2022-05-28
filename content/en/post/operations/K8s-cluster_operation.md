---
title: ESSL-Cluster Operations
author: "Stephan Jagau"
description: ... provides key commands and some pointers for operating the Kubernetes based ESSL-Cluster.
summary: “to be added”
type: docs
weight: 10
to-do-note: "organize ESSL-DataServer using UCI-OAC"
date: "2021-09-09"
draft: true
tags: ["operations", "infra-structure"]
---
###	Operational Kubernetes Cluster
Once the cluster is up and running use the following command to verify operational health:
`kubectl -n kube-system get pods -o wide`

As a result, you should see something like this on your screen:
{{< figure src="../../../../cluster_images/Pods_Kube-System.png" >}}
 
###	Using KUBECTL for Cluster Management
Turn to the `kubectl` reference pages for access to authoritative information on this Kubernetes CLI tool.

The following is a rather arbitrary selection of commands for every-day use:
* `kubectl get cluster-info`
* `kubectl get nodes`
* `kubectl get name-spaces`
* `kubectl -n <name space> get pods [-o wide]`
* `kubectl -n <name space> get services [-o wide]`
* `kubectl -n <name space> get deployments [-o wide]`
* `kubectl -n <name space> get secrets [-o wide]`
* `kubectl describe node/<node name>`
* `kubectl -n <name space> describe pod/<pod name>`
* `kubectl -n <name space> describe svc/<service name>`
* `kubectl -n <name space> describe deploy/<deployment name>`
* `kubectl -n <name space> describe secret/<secret name>`

For additional details concerning the use of the commands listed above please see the _ESSL-Cluster_ administrators.

###	Operations Monitoring
The _ESSL-Cluster_ features both a [Prometheus]({{< ref "prometheus_deployment.md" >}}) and a [Grafana]({{< ref "grafana_deployment.md" >}}) installation. The former is for the gathering of operating data, while the latter uses such data for the creation of near _real-time_ dashboards, which visualize the operating status of the cluster and the applications running in it.

**Note**: As an alternative to _Prometheus_/_Grafana_ the deployment of _ElasticStack_ (ElasticSearch, Logstash, Kibana) could be considered.

###	Data Base and File Back-Up
Since all data of the '_oTree Experiment WorkSpace_' pods is persisted on the ESSL-DataServer, the backing up of all data is matter of saving the relevant files from this server.

**Note**: Check with UCI OAC for details and arrange an appropriate service.

###	Purging of Log Files
All of the relevant log-files are stored in the `/var/log` directory of every ESSL-Cluster and on the ESSL-DataServer node, respectively.

Periodically of said directory and purge all file, the extension of which ends in `*.gz`. As a safety check, verify that one or two digits are preceeding this extension. Use `sudo rm /var/log/*.gz` to remove the log-file archives.

###	Starting and Stopping the ESSL-Cluster
There should never be a situation to power-down the ESSL-Cluster!

Even it a power-outage takes out the entire lab, the ESSL-CLuster should automatically re-start and come back up once power is restored to the lab.

####	Halting the Cluster
If for some reason the _ESSL-Cluster_ needs to be taken down follow these steps:
1. Remove all _Worker_ nodes from cluster by executing: `kubectl delete node name`
1. On each K8s node in turn run: `sudo kubeadm -f reset`
1. On each K8s node perform a controlled _Ubuntu power-down_
1. On the ESSL-DataSever perform a controlled _Ubuntu power-down_

####	Bringing up The Cluster Running
To bring up the ESSL-Cluster when all nodes are powered off, follow these steps:
1. Switch on the ESSL-DataServer and wait for it to have come up running. Verify this by logging in as an _administrator_.
1. Switch on the machine designated the _Kubernetes-Master_ and wait for it to have come up running. Log in as an _administrator_.
1. Run `sudo swapoff -a`
1. Run `sudo rm /etc/cni /opt/cni /var/lib/cni /etc/kubernetes /var/lib/dockershim /var/lib/etcd /var/lib/kubelet /var/run/kubernetes ~/.kube/*`
1. Run `sudo systemctl restart docker`
1. Run `sudo kubeadm init [--v=1|2|3|4|5] --apiserver-advertise-address=164.48.201.220 --pod-network-cidr=192.168.0.0/16 --service-cidr=10.98.0.0/16` and wait for the _Master_ to be up and running.
1. If the previous command has been successful the following will be output >add screenshot<.
1. Perform the three commands that will set up access to the Kubernetes API and enable `kubectl` for use as a _command line_ tool.
1. Now switch on each of the _Kubernetes-Worker_ nodes, wait for them to have come up, log-in, run `sudo swapoff -a`, and then the `sudo kubectl join` command which has been output in step 7.
1. Execute `kubectl get nodes` or `kubectl -n kube-system get pods` to verify all resources on all nodes making up the Kubernetes cluster have been properly initialized.
1. To start the '_container network interface_ (CNI) run `kubectl -f apply /path/to/Calicos/Calico-v.3.16.5.yaml`.
1. After a short wait execute `kubectl -n kube-system get pods` to verify all pods are now up and running.
1. Finally execute `export KUBECONFIG=$HOME/.kube/config`

> include screenshot with output from above command<

In case not all pods have come, wait some time more and then run the `kubectl` command again. If some pods will not come up go to [Kubernetes Trouble Shooting]({{< ref "K8s-cluster_trouble_shooting.md" >}}) for assistance.

*Note*: The data values used in the steps above reflect what has been used in the present _ESSL-Cluster_ set-up.
