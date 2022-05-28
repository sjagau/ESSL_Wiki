---
title: "Update Existing oTree Experimentor Spaces"
author: "Stephan Jagau"
description: "... to start using a different oTree version."
icon: ti-panel
weight: 30
to-do-note: ["draft text for the 'Update an Existing Pod' page", "add summary"]
type: docs
date: "2021-10-03"
draft: false
tags: ["configuration", "deployment"]
---
Several different oTree versions can be used side-by-side in the ESSL-Cluster. It is also possible to change the oTree version without having to delete or move the data asociated with experiments. This enables using the same experiments with different oTree versions.

The following oTree versions are readily available:
* vers-1
* vers-2

Please contact the ESSL-Cluster administrators to have find out if further versions of oTree can be supported.

To activate another version of oTree for an existing _experimentor space_ execute the following command:
* `change-oTree-version.sh <user name> <oTree version>`