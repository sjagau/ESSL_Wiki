---
title: "Kubernetes-Pod for a Specific Application"
author: "Stephan Jagau"
description: ... in broad terms steps through the application agnostic parts of creating the pod.
type: docs
date: "2021-09-09"
weight: 50
to-do-note: "Add a summary to 'Kubernetes-Pod for Specific App' page"
draft: true
tags: ["configuration", "deployment"]
---
## Application Pod Set-Up
The setting up of binary images dedicated to a specific application requires several steps (not necessarily in this order):
* identify which parts of the application are invariant,
* create a Docker or equivalent _container runtime_ (CRT) image,
* provide an NFS volume for persisting data (if required),
* create a suitable external data-base schema (if required),
* define and create all necessary Kubernetes resources,
* decide on runtime configutation parameters (held in _values.yaml_ file), and
* create a Helm chart for the automatic deployment of the application pod.

This text focuses on the actual work steps. For more details and a rationale for the process described herein, please refer to the appropriate documentation by following the links scattered throughout the text.

### Application Analysis
To ensure that the container holding the binary image of the application will be invariant, it is necesary to properly categorize the various components which make up the runtime environment of that application.

Apart from the application binary image, consider the following:
* what network configuration is expected to access the application running in container,
* are the any parameters the values of which need to be supplied when the application is started,
* are files used to persist data across power-down/start-up cycles of the application, and
* is transactional data persistently stored in a data-base schema?

Armed with information about the above, the configuration of the application specific Kubernetes pod may get under way.

### Persisting Pod Data
#### NFS Volume Definition
In order to persist data in files, work with the ESSL-Cluster administrators and have them set up one or more suitable NFS volume on the ESSl-Lab _data storage_ server.

Make sure to obtain a _fully qualified NFS volume_ name, the kind of which starts with `/name`. This name will be needed to specify a Kubernetes _persistent volume_ resource for use by the application specific pod.

#### Data-Base Schema for Persisting Data
Similarly, unless data-base transaction data will need to be stored in the application-specific pod only, it may be necessary to have a data schema set up in an external _data base management system_.

Again, this is best done by the ESSL-CLuster administrators. Be sure to obtain a _schema-name_ URL, a user-id, and a matching password so that the created data-schema is accessible from inside the application pod being deployed.

At the time of writing the ESSL-Lab provides support for `PostgreSQL` only.

###	Docker Image Preparation
The heart and core of the application specific pod will be a binary _container runtime_, usually a _Docker_ image.

For an in-depth discussion of Docker and how to use it, please consult the **[Docker Documenation](https://docs.docker.com)** web-site.

####	Access to a Docker Registry
In order to use a Docker or equivalent _container runtime_ binary image in a Kubernetes pod, it will be necessary to stage the image in a suitable registry. Read the page [Docker Registry Overview](https://docs.docker.com/registry/) if you are unfamiliar with the subject.

Also, check with the ESSL-Cluster administrators for advice on where to stage your application binary image and how to do that.

####	Creation of Docker Images
To build a _containerizable_ binary image of the target application, a so called _Dockerfile_ needs to be created. Work with the ESSL-Cluster administrators or consult the [Writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) web-page for details on how to create a _Dockerfile_.

Once the Dockerfile is ready for testing run:

`docker build -t repo-name/image-name:tag`

To verify the image has been created and is executable run the following two commands:

```
docker images
docker exec -it name-of-the-image /bin/bash
```

The second command assumes that a _Bash_ shell executable is available in the _Docker-built_ binary image.

To run a container as a servive, which exposes both a shell and a web-application, enter:

`docker run -dt -p ext-port:int-port`

where `int-port` refers to port that the application inside the container is listening to. Also, depending on the container set-up, more than one external/internal port pair may be specified. Here do make sure that port numbers reserved for a service, like `22` for the _ssh-server_, are used for the value of `int-port` parameter.

**Note**: All of the above may have to be prefixed with `sudo` depending on user set-up.

###	Helm Chart for Application Specific Kubernetes Pod
The primary purpose of Helm charts in the ESSL-Cluster is to keep track of the various deployments.

As a result the Helm charts used in this context are rather simple and consist mostly of _deployment_, _service_, _configMap_, and _secrets_ manifests. Consequently, the use of _values.yaml_ files is more of a convenience than a necessity.

For further details concerning Helm and Helm charts consult the [Helm documentation](http://localhost:1313/404.html).

####	Creation of Application-Specific Helm Charts
In association with the binary image underlying the container runtime of the application, at a minimum the following Kubernetes resource manifests are required:
* a _deployment_ manifest describing the application-specific container,
* a _service_ manifest specifying how the application can be accessed across the network, and
* (optionally) a _configMap_ manifest to deal with the runtime configuration parameters required by the application.

Describing in detail the individual elements of required Kubernetes resources mentioned above is beyond the scope of this documentation. For further details either study the ESSL-Lab '_[oTree Experiments Work-Space]()_' Helm chart or contact the ESSL-Cluster administrators for advice and help.

#### Service Exposable by Application Specific Pod
The ESSL-CLuster has been configured to support all three service access methods available for a Kubernetes pod: _NodePort_, _ClusterIP_, and _LoadBalancer_.

Depending on the make-up and configuration of the underlying _container runtime_ image any one of the three alternatives may be eligible for use with the services exposed by the application specific pod.

Consult with the ESSL-Cluster administrators for advice which service access method is most suitable for the application on hand.

### Application Specific Pod Start-Up and Access
Once the _container runtime_ image has been pushed to a Docker registry and the Helm chart has been created, rum the `helm install` command with the `--dry-run` option to make sure the Helm chart is error free.

Also, it may be useful to later on run the actual `helm install` command with the `-f name-of-the-values.yaml-file` option. This way several instances of the _application specific_ pod can be started up, each one with its own individual runtime configuration.

**Note**: During the _pod testing_ phase it may be expedient to work closely with the ESSL-Cluster administrators so as to leverage their previous experience from building application specific Kubernetes pods.

### Application Specific Pod Running on the ESSL-CLuster
####	Work-Space for oTree Experiments
Work-space for oTree Experiments consists of the following components:
| Name | Version | Comments / Details |
|----|----|----|
| OS-Name Image | 9.99 | Docker image with: |
|  |  | curl, vim, wget tools |
|  |  | Python 9.99 and PIP |
| oTree | 9.99 |  |
| Redis | 9.99 | colocated in pod |

In addition to the components resident in the pod, a PostgreSQL instance and an NFS volume deficated to the oTree pod are configured on the ESSL-DataServer.

Check with the ESSL-Cluster administrators for further details.

####	Hugo-Based ESSL-Cluster Documentation
This documentation is being generated by a _Hugo-Extended_ server container running in the _ESSL-Cluster_ with NFS file access to the _ESSL-DataServer_.

The _container runtime_ used is the `jakejarvis/hugo-extended` Docker container.

The NFS file volume is mapped to `/path/to/hugo/contents/` on the _ESSL-DataServer_.

**Note**: Please contact the ESSL-Cluster administrator in case you will happen across an error in this documentation or with any suggestions for improving the same.