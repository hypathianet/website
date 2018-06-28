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

A Hypatha.net cluster is constructed from the following main components:

* [Gitlab](https://about.gitlab.com) as main user interface for group management and version control, collaboration and communication. It also acts as the source of truth for user permissions and artifact ownership. While the Enterprise version offers useful additional functions, for operating a Hypathia-style cluster, the open-source version is sufficient.

* [Kubernetes](https://kubernetes.io) in a multi-tenant configuration for orchestrating containers based applications. The cluster runs on a number of worker nodes. Each node runs the pods necessary to be the master node, but only one will be active at any time (selected by Kubernetes via a [Leader Election](https://en.wikipedia.org/wiki/Leader_election). Some of the worker nodes also operate elements of the etcd cluster to hold the state of the Kubernetes cluster. Other nodes may have special hardware, like dedicated GPGPU cards.

* Container Linux as operating system for nodes. The current phase of restructuring of the project makes it unclear whether the [version offered by CoreOs](https://coreos.com/os/docs/latest/), the division of RedHat Inc. or by [Flatcar](https://www.flatcar-linux.org/), the version forked by the original authors is more ideally suited. We will keep you posted.

* [Ceph](https://ceph.com/) as storage solution. This is provided to an Hypathia K8s cluster via the [Rook](https://rook.io/) project.

* LDAP for password and identity management. This can be provided via a number of solutions.

Currently there are two sets of service components that are available twice each: the image registry and the DHCP server. Both have sets of features, that are not completely covered by either implementation and thus both are needed to operate the Hypathia.net installation.