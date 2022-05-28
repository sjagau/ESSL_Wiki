---
title: "Kubernetes FLUX Plug-In"
author: "Stephan Jagau"
description: "... provides an overview on how to set-up and operated FLuX"
icon: ti-panel
weight: 30
to-do-note: ["section structure"]
type: docs
date: "2022-05-28"
draft: false
tags: ["configuration", "deployment"]
---
This command links the GitHub repo to the FlUX system:
```
flux create image repository hugo-site \
  --image=codebird/essl-docs \
  --interval=1m \
  --export > ./clusters/my-cluster/hugo-site-registry.yaml
```

This command sets the updata policy:
```
flux create image policy hugo-site \
  --image-ref=hugo-site \
  --select-semver=2.0.x \
  --export > ./clusters/my-cluster/hugo—site-policy.yaml
```

This command applies the update policy:
```
flux create image update flux-system \
  --git-repo-ref=flux-system \
  --git-repo-path="./clusters/my-cluster" \
  --checkout-branch=main \
  --push-branch=main \
  --author-name=codebird \
  --author-email=codebird@users.noreply.github.com \
  --commit-template="{{range .Updated.Images}}{{println .}}{{end}}" \
  --export > ./clusters/my-cluster/flux-system-automation.yaml
```