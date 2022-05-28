---
title: Connect ESSL-Cluster Pods to PostgreSQL-Server
author: "Stephan Jagau"
description: Outlines how an application running in an _ESSL-Cluster_ pod can be connected to a PostgreSQL DBMS instance on the _ESSL-DataServer_.
summary: “Outlines the setting up of a PostgreSQL servier on the _ESSL-DataServer_ and how to connect to it.”
type: docs
weight: 10
to-do-note: "none"
date: "2021-05-13"
draft: true
tags: ["configuration", "persisting-data"]
---
### PostgreSQL Server Configuration
For instructions to install and configure a data-base server in support of the oTree experiments follow the [otTree Postgres Data-Base](https://ibsen-otree-docs.ibsen-h2020.eu/server/ubuntu.html#database-postgres) page.

Start the data-base server running by entering `pg_ctlcluster 13 main start`.

Login by using the following commands:
```
sudo su - postgres
psql
```

Use these commands to create a _Django_ data-base schema:
```
CREATE DATABASE django_db;
CREATE USER otree_user WITH PASSWORD 'mypassword';
GRANT ALL PRIVILEGES ON DATABASE django_db TO otree_user;
```

Note these deployment specific data-items:
```
DB: otree_data
UID: otree_user
PWD: otree
```

### PostgreSQL Server Environment Configuration
Verify the meaning and neccessity of the following configuration steps:
* `/etc/postgresql/13/main/pg_hba.conf \\ postgresql.conf`
* `sudo systemctl daemon-reload`
* `sudo service postgresql status`
* `sudo lsof -i -P -n | grep LISTEN`