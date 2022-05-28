---
title: ESSL-Cluster Infra-Structure
author: "Stephan Jagau"
description: ... describes key facts characterizing the HW underlying the ESSL-Cluster.
summary: “to be added”
type: docs
weight: 50
to-do-note: "add summary to 'ESSL-Cluster Infra-Structure' page"
date: "2021-09-09"
draft: true
tags: ["configuration", "infra-structure"]
---
### ESSL-Cluster
The Kubernetes cluster in the ESS-Lab at UCI is based on [currently] file Dell server machines all running Ubuntu for their operating system. Each machine has at least one _rotating_ disk-drive attached to it.

####	Dell Server Details
Most of the information shown about individual machines in the sections below has been extracted by using the `kubectl describe node/node_name` command.

#####	K8s-MASTER
```
Host Name: C16080100.ss2k.uci.edu
IP Address: 128.200.135.194
MAC Address: 18:66:DA:29:6F:4B
Dell Model: Precision Tower 3620
——————————
Linux 4.19.0-11-amd64 x86_64
PRETTY_NAME="Debian GNU/Linux 10 (buster)"
NAME="Debian GNU/Linux"
VERSION_ID="10"
VERSION="10 (buster)"
VERSION_CODENAME=buster
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
Ubuntu® 18.04 LTS
```

#####	K8s-WORKER--1
```
Host Name: C11100502.ss2k.uci.edu
IP Address: 128.200.135.203
MAC Address: 18:03:73:B6:2F:C6
Dell Model: Optiplex 990
————————
Linux 4.19.0-8-amd64 x86_64
PRETTY_NAME="Debian GNU/Linux 10 (buster)"
NAME="Debian GNU/Linux"
VERSION_ID="10"
VERSION="10 (buster)"
VERSION_CODENAME=buster
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
Ubuntu® 18.04 LTS
```

#####	K8s-WORKER--2
```
Host Name: C16080102.ss2k.uci.edu
IP Address: 128.200.135.204
MAC Address: D4:AE:52:93:F2:8D
Dell Model: Optiplex 7040
Processor: Intel Core(TM) i5-7600 CPU @ 3.50GHz 
————————
Linux 4.19.0-8-amd64 x86_64
PRETTY_NAME="Debian GNU/Linux 10 (buster)"
NAME="Debian GNU/Linux"
VERSION_ID="10"
VERSION="10 (buster)"
VERSION_CODENAME=buster
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
Ubuntu® 18.04 LTS
```

#####	Data-Server
```
Host Name: C13031900.ss2k.uci.edu
IP-Address: 128.200.135.220
MAC Address: D4:8E:38:96:E5:FC
Dell Model: PowerEdge T310
————————
Ubuntu® 18.04
```

#####	Additional Lab Hardware
There is one more machine in the ESSL-Lab that may become yet another Kubernetes worker node:
```
Host Name: C16080101.ss2k.uci.edu
IP Address: 128.200.135.195
MAC Address: F4:8E:38:96:E2:63
Dell Model: Optiplex 7040
————————
```

####	Ubuntu Operating System
For all of the nodes listed below Ubuntu has been chosen for an operating system.

Before this back-drop please note the data in the table below as an aid to obtain maintenance support for the operating systems deployed on the Dell machines:
|   | Released | End of Life |
|----|----|----|
| Ubuntu 14.04 LTS | Apr 2014 | Apr 2019 |
| Ubuntu 16.04 LTS | Apr 2016 | Apr 2021 |
| Ubuntu 18.04 LTS | Apr 2018 | Apr 2023 |
| Ubuntu 20.04 LTS | Apr 2020 | Apr 2025 |
| Ubuntu 20.10S | Oct 2020 | Jul 2021 |

At the time of this writing all nodes have `Ubuntu 18.04 LTS` installed.

For the `Dell Optiplex 990` the `Ubuntu 16/14.04 LTS` release may have to be used despite such systems having reached their _End-of-Life_ dates already.

Check the page [Ubuntu Releases](https://releases.ubuntu.com) for down-load links of the versions under consideration.
