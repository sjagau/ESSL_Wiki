---
title: "Deploy and Configure REDIS Cluster"
author: "Stephan Jagau"
description: "Deploy a REDIS cluster in a Kubernetes cluster using an existing K8s StorageClass"
type: docs
icon: ti-panel
weight: 50
to-do-note: "Add a summary to 'Deploy and Configure REDIS Cluster' page"
date: "2021-10-03"
draft: true
tags: ["configuration", "operations"]
---

### 'REDIS Cluster' Deployment Steps
1. `kubectl create namespace redis`
2. `kubectl -n redis apply -f /path/to/redis-configmap.yaml`
3. `kubectl -n redis apply -f /path/to/redis-statefulset.yaml`
4. `kubectl -n redis get pods` -- check whether the Redis-Master is coming up
5. `kubectl -n redis get pv` -- check whether the Redis storage is set up
6. `kubectl -n redis logs redis-0` -- Redis-Master should perform replication
7. `kubectl -n redis logs redis-1` -- Redis node connecting to Redis-Master

### Verify 'REDIS Cluster' Deployment
1.  `kubectl -n redis exec -it redis-0 -- /bin/bash` -- check replication results
2. Run `redis-cli` and authenticate
3. Against REDIS `>` prompt run `info replication`

### 'REDIS Sentinel' Deployment Steps
1. `kubectl -n redis apply -f /path/to/sentinel-statefulset.yaml`
2. `kubectl -n redis get pods` -- check whether the Sentinal pods are coming up
3. `kubectl -n redis logs sentinel-0` -- Sentinel should be monitoring the Redis-Master

### Test 'REDIS-Cluster' Fail-Over
Begin by verifying the handling of a master node failure
1. `kubectl -n redis get pods` -- to see all Redis nodes
2. `kubectl -n redis delete pod redis-0` -- remove Redis-Master node
3. `kubectl -n redis logs sentinel-0` -- look for switch on master node
4. `kubectl -n redis exec -it redis-x -- /bin/bash` -- 'redis-x' is the new master
2. Run `redis-cli` and authenticate
3. Run `info replication` to check if 'redis-x' is indeed the master now
4. `kubectl -n redis logs redis-0` -- check whether 'redis-0' sees the new master

Now check the handling of the Sentinel failing
1. `kubectl -n redis delete pod sentinel-0` -- remove Sentinel node
2. `kubectl -n redis logs sentinel-0` -- check if Sentinel node is properly coming back

### Connect to REDIS-CLUSTER
To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace essl-test redis-cluster -o jsonpath="{.data.redis-password}" | base64 --decode)

To connect to your Redis server:

1. Run a Redis pod that you can use as a client:

   kubectl run --namespace essl-test redis-cluster-client --rm --tty -i --restart='Never' \
    --env REDIS_PASSWORD=$REDIS_PASSWORD \--labels="redis-cluster-client=true" \
   --image docker.io/bitnami/redis:5.0.7-debian-10-r32 -- bash

2. Connect using the Redis CLI:
   redis-cli -h redis-cluster-master -a $REDIS_PASSWORD
   redis-cli -h redis-cluster-slave -a $REDIS_PASSWORD


Note: Since NetworkPolicy is enabled, only pods with label
redis-cluster-client=true"
will be able to connect to redis.
