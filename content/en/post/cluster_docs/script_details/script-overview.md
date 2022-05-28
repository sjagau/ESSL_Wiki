---
title: ESSL-Cluster Management Scripts
author: "Stephan Jagau"
description: ... contains scree-shots of all of the scripts used to automate the management of ESSL-Cluster users.
summary: “Pictures of scripts used for managing the ESSL-Cluster users.”
type: docs
weight: 60
to-do-note: "add summary to 'ESSL-Cluster Infra-Structure' page"
date: "2021-09-09"
draft: true
tags: ["configuration", "operations"]
---
This page shows pictures of the scripts used for managing the experimentor users of the ESSL-Cluster.

The pictures are organized into a _Kubernetes Master_ and a _Data Server_ section. Pictures referring to a _directly callable_ script are marked.

### Scripts on _Kubernetes Master_ Node
This is the `oTree-pod-creation.sh` script that gets executed for setting up a new _experimentor_ work-space:
{{< figure src="../mstr_pod_create.png" >}}

Should an experimentor request to use another oTree version, the `change-oTree-version.sh` script shown here gets executed:
{{< figure src="../mstr_chng_oTree_vers.png" >}}

The `first-deactivate-user.sh` script shown below is used to suspend the operations of a particular _experimentor work-space_:
{{< figure src="../mstr_1-dact_usr.png" >}}

With the `reactivate-user.sh` script shown next a suspended _experimentor work-space_ can be resumed:
{{< figure src="../mstr_ract_usr.png" >}}

Lastly, the picture below shows the `second-delete-user.sh` script which deletes an _experimentor work-space_ and all data files associated with it:
{{< figure src="../mstr_2-del_usr.png" >}}

### Scripts on _Data Server_ Node
The scripts pictured in this section are stored and executed on the ESSL-Cluster _Data Server_. They never called directly but their execution is triggerd by scripts on the _Master Server_.

To start, the `user-setUp.sh` script creates a user on the _Data Server_ machine along with a home directory and some management scripts contained therein:
{{< figure src="../../../cluster_images/ScreenshotPlaceholder.jpg" >}}

The contents of the `key-message.txt` file gets used in an E-mail sent to the newly created _experimentor user_ as a greeting text:
{{< figure src="../d-svr_key-msg_txt.png" >}}

The `db-create.sh` script creates a data-base schema in the _PostgreSQL_ data-base and assigns it to the _experimentor user_ being set-up:
{{< figure src="../d-svr_db-create.png" >}}

Similarly, the `db-delete.sh` script gets used to delete the data-base schema of an _experimentor user_ from the _PostgreSQL_ data-base:
{{< figure src="../d-svr_db-create.png" >}}