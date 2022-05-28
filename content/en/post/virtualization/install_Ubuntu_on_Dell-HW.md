---
title: "Install Ubuntu on Dell-HW"
author: "Stephan Jagau"
description: ... provides important information that helps to install Ubuntu on the Dell machines available in the ESSL-Lab.
to-do-note: "Based on Word document: Part 1 - Install Ubuntu on Dell-HW"
date: "2021-09-06"
type: docs
draft: true
weight: 30
tags: ["configuration", "deployment"]
---
This exposé has three sections:
* preparing the Dell HW for loading Ubuntu from a memory stick,
* loading and configuring the operating system, and
* customizing the network settings.
## Preparing Dell Hardware
The various Dell model machines have disparate approaches to entering a mode where an external medium can be read to create an instance of an operating system on the mass storage device (HDA or SSD) built into the machine.

In the following separate sections are dedicated to each of the Dell models considered.

### Preparing Precision Tower 3620
The following URL point to either web-pages or YouTube videos explaining the Ubuntu installation process with a disk data-wipe as it is applicable to the Precision Tower 3620:

## Ubuntu Configuration
The various Dell model machines have disparate approaches to entering a mode where an external medium can be read to create an instance of an operating system on the mass storage device (HDA or SSD) built into the machine.

In the following separate sections are dedicated to each of the Dell models considered.

### Preparing Dell Precision Tower 3620
The following URLs point to either _YouTube_ videos or web-pages explaining the operating installation process applicable to the Precision Tower 3620:

* [How to Install Ubuntu from USB](https://www.youtube.com/watch?v=RW9UWDOJjL4) (official Dell Tech Support)

* [How to Install Ubuntu Server](https://www.youtube.com/watch?v=0otNmOA2hWc) (focusses on Dell Power Edge)

* [How to Install Ubuntu 18.04 on Dell Laptop](https://www.youtube.com/watch?v=6p_2fRdExMs) (added for boot sequence handling)

* [A Clean Install Linux Ubuntu 20.04](https://www.youtube.com/watch?v=n8VwTYU0Mec) (focusses on using Dell UEFI BIOS)

* _[Updating the BIOS](https://dellwindowsreinstallationguide.com/uefi/updating-the-bios/)_ (focusses on Windows, equally applicable to Ubuntu)

* _[Anleitung zum Installieren von Ubuntu]()_ (dual boot mit Windows)

(**Note**: The URLs of Web-Pages are set in _Italics_)

### Preparing Dell Optiplex 7040
Check the preceeding section for information explaining how to install Ubuntu on a Dell Optiplex machine.

### Preparing Dell Optiplex 900
The following URL points to a YouTube video explaining the operating installation process applicable to the _Optiplex 900_:

* [Installing Ubuntu 18.04 "Bionic Beaver"](https://www.youtube.com/watch?v=KtOrBL2XIXs) (came up in response to Optiplex 900 search)

In addition to the one listed above the YouTube videos and web-pages in the first section above are also applicable.

## Network Settings Customization
After the machine has rebooted for the first time, go ahead and customize the Ubuntu network configuration by creating the following contents in the file `/etc/netplan/00-installer-config.yaml` (note, the name of the file may differ):
```
network:
  version: 2
  ethernets:
    <machine Ethernet port>:
      addresses: [ip-adr/26]
      gateway4: 128.200.135.193
      nameservers:
        addresses: [128.200.18.222, 128.195.133.145]
```

Do take note of the indentation which adds two spaces for each new level. The content above has five (5) indentation levels.

After saving the file, reboot!

## Post-Installation Chores
After the network customization reboot, apply the following changes in preparation of the subsequent Docker and Kubernetes installations.

### Prevent Server Hibernation
For nodes to receive a Kubernetes configuration for the first time, ensure that the machine behaves like a server: it should never go into sleep or hibernation mode.
To prevent this from happening run:

`sudo systemctl mask sleep.target suspend.target \ hibernate.target hybrid-sleep.target`

### Docker and Kubernetes Preparaton
See the Kubernetes Step-by-Step document for Docker and Kubernetes specific system-level preparations.