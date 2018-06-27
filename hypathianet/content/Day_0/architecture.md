+++
date = "2018-06-27T11:40:18+02:00"
weight = 200
description = "Architecture Overview: Components from a fairly high level"
title = "Architecture"
draft = false
toc = true
+++


## Architecture Overview

{{< figure src="/img/docs/introduction/Hypathia_Architecture.svg" title="Hypathia.net Architecture" width="70%">}}

A Hypatha.net cluster is constructured from the following main components:

* a Gitlab instance to hold the source and configuration data of the projects to be run on the cluster. It also acts as user interface for users to create projects and organize users into groups
* the actual Kubernetes cluster, running on a number of worker nodes. Each node also runs the pods neccesary to be the master node, but only one will be active at any time. Some of the worker nodes also operate elements of the etcd cluster to hold the state of the Kubernetes cluster.


