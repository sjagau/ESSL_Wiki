---
title: "APT Package Manager"
author: "Stephan Jagau"
description: "Summary of 'apt' package manager commands as used in Debian/Ubuntu."
type: docs
icon: ti-panel
weight: 10
to-do-note: []
date: "2021-10-23"
draft: true
tags: ["installation", "generic"]
---
This page lists the Debian/Ubuntu `apt` package manager commands:

| 'apt' command | 'apt-get' command | synopsis |
|----|----|----|
| apt install | apt-get install | install packages |
| apt remove | apt-get remove | remove packages |
| apt list | dpkg list | list packages |
| apt list --upgradable | -- | list pending updates |
| apt purge | apt-get purge | remove packages and configuration |
| apt update | apt-get update | update repository |
| apt upgrade | apt-get upgrade | update pending packages |
| apt full-upgrade | apt-get dist-upgrade | update and de-install pending packages |
| apt autoremove | apt-get autoremove | remove packages no longer needed |
| apt search | apt-cache search | search for packages |
| apt show | apt-cache show | display package details |
| apt edit-sources | -- | edit sources.list |