---
title: "OS Environment Configuration"
author: "Stephan Jagau"
description: ... addresses key aspects of the Dell/Ubuntu environment chosen for the creation and operation of the ESSL-Cluster.
type: docs
weight: 10
to-do-note: ["add summary", "complete formatting", "relocate content"]
date: "2021-09-06"
draft: true
tags: ["configuration", "pending completion"]
---
## Overview
### Introduction
This is not a textbook, rather this file collects succinct descriptions and pointers for operating a Kubernetes cluster.
There are a lot of sections in this document (see above) which will be fleshed out as we will go along with the cluster definition and set-up on the one hand or environment preparations on the other.
While the main text focusses on the individual steps to be taken, several appendices will provide background and more detailed information.

### Assumptions
The following assumptions are used to guide the setting up of the Kubernetes cluster:
•	the physical nodes (machines) supporting the Kubernetes cluster cannot be reached directly across the Internet,
•	all external connections are terminated on Kubernetes services,
•	the data-base and file-store machine uses an IP address that allows for the Kubernetes physical nodes to connect to it,
•	the NFS connections are not secured (physical Kubernetes nodes trust data-base/file-store node).

### Basic Planning Steps
Prior to effecting the set-up of the Kubernetes cluster several general planning steps should be documented as guide for pending work:
•	determine the purpose of each physical node,
•	record currently used IP addresses and existing config-data,
•	taking into consideration the previous set-up, create an IP address plan for physical (server) nodes, the data-base/file-store, the Kubernetes service end-points, and the work-stations.

###	Next Steps
•	detail the DHCP data base back-up for relocating to a new machine, setting up the service on the new machine, verifying it working, and subsequently purging DHCP from the original node
•	preparing three machines for Kubernetes installation

##	Application to UCI ESSL
this chapter will address the specific of the UCI lab where a Kubernetes cluster will be set up
this chapter is not for general use

##	Preparations
These instructions are applicable to Debian and Ubuntu.

There may be slight modifications of the command required if CentOS or Linux are used. It is left to the reader to figure out which changes need to be made.

###	Prepare OS Boot-Stick
UNetbootin	http://unetbootin.github.io/#install is an easy to use app for creating boot-media like a ‘`boot stick`’.

The program can be used to download the appropriate (Debian / Ubuntu) ISO image. Alternatively, `UNetbootin` will also read a local ISO image.

Following screenshot _Ubuntu-18.04-P23_ a list of SW packages is displayed.

Make sure 'Open SSH Server' is selected before continuing the installation process.
_P24: install Open-SSH server_

Screenshot _Ubuntu-18.04-P28_ shows an example for how to set up the configuration file in '`/etc/netplan~'

###	Verify Presence of Tools
apt, git, curl, arp-scan

###	DHCP Re-Installation
sudo apt-get install isc-dhcp-server

https://www.hiroom2.com/2017/06/27/debian-9-install-dhcp-server/

DHCP (_Dynamic Host Configuration Protocol_) is used to automatically configure hosts with crucial IP parameters across the network when they are booted up. This way machines on a network will have addresses drawn from a pre-defined range, will know which default router to use, and where to find the DNS servers.

In some cases DHCP is also used to configure hosts for Windows-based IP communications.

This section has been created using the Debian DHCP Server Configuration web-page as a guide. Some details have been taken from the Debian DHCP Server wiki-page.

For in-depth administration guidelines check out the Oracle’s Network Address Configuration manual.

Notes:
* save existing configuration depending on current implementation
* `https://www.der-windows-papst.de/wp-content/uploads/2019/07/Windows-Server-2019-DHCP-Server-einrichten.pdf`
* see `/usr/share/doc/dhcp*/dhcpd.conf.sample` and `https://en.wikipedia.org/wiki/IP_address`
* DHCP Manual Page 
* Network Diagnosis Tools (chapter 10.9 of the ‘Debian Administrator Handbook’)
* DHCP configuration file ‘/etc/dhcp/dhcpd.conf’ explained
* Debian 9 Network Settings covers the basic settings required on all clients and/or servers
* Debian wiki DHCP Server set-up, use the references to the `/etc/network/interfaces`, `/etc/resolv.conf`, `/etc/default/isc-dhcp/server`, and `/etc/dhcp/dhcpd.conf` files as additional sources of information
* there are other files in `/etc/` which hold information about the Ethernet boards and ports configured `ls /sys/class/net`, `/etc/iftab`, 
`/etc/default/isc-dhcp-server` for IPV4

####	Saving Existing DHCP Data-Base
On Ubuntu the DHCP configuration file is at /etc/dhcp/dhcpd.conf.
For Windows-Server look in: %SystemRoot%System32\DHCP\backup
Display the file contents to make sure the contents are what you are looking for in the context of your network. You should expect data similar to this:
{{< figure src="../dhcp_config_example.png" >}}
 
Transfer the DHCP content to a storage area where it will be accessible for loading into the targeted new DHCP server.

####	Verify Availability of DHCP on Debian Target Host
This section has been created using the Debian DHCP Wiki-Page web-page as a guide.
To check whether DHCP is already available run:
sudo service isc-dhcp-server status
To check whether DHCP is already available run:
sudo service isc-dhcp-server status

####	Configure DHCP Data-Base
text

####	Verify DHCP Server Running
On the server running the DHCP service confirm that the service is indeed up and running:
`sudo service isc-dhcp-server status`

Make sure the output from the command is indicating that the service is _running_ or _active_.

Next go to a workstation, which is supposed to receive its IP configuration from the DHCP server.

####	Configure Debian DHCP Clients
Boot up the Debian-Client.
Open the file `/etc/network/interfaces` and verify that its contents look like this:
```
auto lo eth0
auto lo inet loopback
iface eth0 inet dhcp
```
Instead of `eth0` physical port names like `enp0s31f6` or `enp0s25` may also be showing.

If not make the necessary changes and re-boot the machine.

Once the machine is up and running, log-in and verify its IP configuration by running: ifconfig

Make sure the output matches what has been configured on the DHCP server. In particular check the IP address shown for the DHCP and DNS servers as well as the default router.

####	Configure Windows DHCP Clients
{{< figure src="../../../cluster_images/ToBeAdded.png" >}}

###	NFS Server and Client Installation
The NFS set-up is such that the hosts making up the nodes of the Kubernetes cluster all trust each other.

To prevent un-authorized hacker access, all users of containerized applications will be routed via Kubernetes service end-points and their access will be controlled at the service level.

####	NFS Server Set-Up and Configuration
{{< figure src="../../cluster_images/ScreenshotPlaceholder.png" >}}

####	NFS Client Configuration
In the Docker OS base-image, make sure that ‘systemd’ is present and active. Also include ‘_nfs-common_’ and '_cifs-utils_' in the packages that are used to build the OS image.

In the running container you should see something like the following:
{{< figure src="../../../cluster_images/Screenshot_Placeholder.png" >}}