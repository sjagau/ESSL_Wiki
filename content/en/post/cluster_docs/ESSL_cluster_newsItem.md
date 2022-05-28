---
title: "ESSL-Cluster Announcement"
author: "Stephan Jagau"
description: contains the text used to inform the UCI ESSL community.
summary: "contains the campus press announcement of the ESSL-Cluster having come on-line."
type: docs
weight: 10
to-do-note: "none"
date: 2021-09-06T13:55:30+02:00
draft: true
tags: ["generic"]
---

### ESSL-Cluster is now Available

We used the ongoing shutdown of the ESSL facilities at UCI to virtualize the lab server using state-of-the-art Kubernetes-based cluster management technology. The new ESSL server features fully separated individual oTree-workspaces, that are remotely accessible 24 hours, 7 days a week for all researchers.

Once you have your ESSL workspace, you can interact with it from your laptops (similar to a remote directory). This allows conducting experimental projects from writing the first bits of code all the way to running sessions – all within the same workspace.

_oTree_ and _Python_ are loaded into the workspaces as a Docker image from the ESSL’s docker-hub repository, whereas throughout the lifespan of a workspace all your code and all user data are safely stored on a separate database on a server located in the ESSL lab. As a unique added feature, this allows on-demand customization of the _oTree_- and _Python_-versions running in each workspace (currently supporting **oTree v2.0-5.2**). For your existing experimental projects, this means that you will never have to update oTree-code after starting to run sessions, and you can run extensions and replications using the exact software environment that was current when you started running a given experiment.

Subjects can access the new oTree webservers within and without the UCI-intranet, which makes them equally suitable for experimentation at the ESSL-facilities on campus and for online/off-campus experimentation with UCI- or non-UCI subjects.

The new server has been implemented by Stephan Jagau (Postdoctoral Scholar, IMBS) and Prof. John Duffy with support from Social Sciences Computing Services.

Sign up to get your own oTree workspace at the ESSL today (mailto:ESSL@uci.edu)! The self-guided installation is easy and takes about 15 minutes.
