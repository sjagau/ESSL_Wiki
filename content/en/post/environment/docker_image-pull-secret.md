---
title: "Creation of Docker Image Pull Secret"
author: "Stephan Jagau"
description: ... explains how a Kubernetes Secret resouce is created that enables the pulling of container runtime images from arbitrary registries.
type: docs
date: "2021-09-15"
weight: 10
to-do-note: "none"
draft: true
tags: ["configuration"]
---
For Kubernetes to be able to pull _container runtime_ images from registries an _image pull secret resource_ is required.

The following assumes that the user has identified a target Docker registries and has logged in to it using the [Docker Log-In](https://docs.docker.com/engine/reference/commandline/login/) command.

Once the log-in to the target registry is active, the following command must be executed to create the _Docker Image Pull Secret_ as a Kubernetes resource:

```
kubectl create secret generic regcred \
   --from-file=.dockerconfigjson=path-to docker-secret \
   --type=kubernetes.io/dockerconfigjson
```

The Docker secret may be stored either in:

`/root/docker/config.json`

or in

`$HOME/.docker/config.json`

Use `kubectl get secrets` to verify that the secret has been created.

**Note**: All of the above _Docker_ commands may have to be prefixed with `sudo` depending on user set-up.
