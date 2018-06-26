+++
title = "Day 3: Maintenance"
description = "Maintaining the Cluster indefinitely."
draft= false
weight = 400
+++

A Kubernetes cluster needs constant upkeep. Not only is the development of Kubernetes progressing in a rapid fashin, making frequent updates neccesary, but also a lot of components improve at a similar break-neck speed, with their own rapid release shedules.

Most of the upkeep of the applications running within the cluster are either automatic through the versioning of the running container images. This should require no manual work by the administrator. Even the users running these apps should strive to automate the updating of application containers as much as possible.

But the infrastructure components that make up Kubernetes may require more attention and even manual intervention.

