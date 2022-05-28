---
title: "Manage oTree Experimentor Spaces"
author: "Stephan Jagau"
description: ... describes the steps necessary to use prepared scripts for the creation, suspension, and deletion of an oTree experimentor application space.
type: docs
date: "2021-11-26"
weight: 20
to-do-note: "Clone the oTree application pod template using available scripts"
draft: true
tags: ["configuration", "deployment"]
---
The creation of a new oTree application environment for another experimentor is straightfoward as it is based on a number of re-usable templates and shell scripts.

### Gather Information about Experimentor and Desired oTree Version
Prior to executing the scripts that will create the oTree application space for the new experimentor, the following information should be kept on hand:
* user-name of the new experimentor (one word, max. XX chars, all lowwer case)
* unless the experimentor is a UCI staff member, E-mail address of experimentor
* oTree version requested by the new experimentor

### Scripts to Create oTree Application Space
**Note**: The scripts mentioned in this section execute on the ESSL-Cluster _Master Server_. They do, however, call scripts on the ESSL-Cluster _Data Server_ for the creation of the _experimentor user_ and the PostgreSQL data-base schema assigned to the _experimentor user_.

On the _Master Server_ execute the following script:
* `oTree-pod-creation.sh <user-name> <oTree version>`

Apart from creating the experimentor user, the scripts executing on the _Data Server' will also trigger the sending of an E-mail message informing the new user of the oTree work-space being ready for use.

### Script to Suspend Operations of an oTree Application Space
To temporarily suspend operating a specific oTree application space run:
* `first-deactivate-user.sh <user-name>`

#### Resume a Suspended oTree Application Space
To resume operations of a suspended oTree application space execute:
* `reactivate-user.sh <user-name> <oTree version>`

#### Delete an oTree Application Space for Good
A suspended oTree application space will get deleted by running:
* `second-delete-user.sh <user-name>`

Before exeuting the script mentioned above, make sure that all user data on the NFS server has been backed up.

**Note**: Please contact the ESSL-Cluster administrators in case you will happen across an error in this documentation or with any suggestions for improving the same.