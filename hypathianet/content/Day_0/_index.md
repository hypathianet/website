+++
date = "2018-05-17T06:30:49+02:00"
weight = 1
description = "Introduction to hypathia.net including mission statement and scope of the project."
title = "Day 0: Introduction"
draft = false
bref= "hypathia.net is a Kubernetes stack aimed at providing computing resources for academic institutions. This requires accepting an operating environment and forms of resource requests that differ somewhat from the more conventional deployment of a single cloud scale application."
toc = true
+++

### Mission

Hypathia.net aims to provide a blueprint for operating a service to provide
cloud based computing resources in a self-service manner. It presents an opinionated
selection of components, configurations and practices tailored to environments that
have a specific set of requirements, often found at academic institutions.

Hypathia.net also wants to become a hub for a community of those that operate Kubernetes clusters that are constructed from this blueprint. Thus any feedback, initially via the issues or better, pull-requests to the [GitHub Project]()
All documents on the site will be provided in multiple languages, initially English and German. More if somebody volunteers to provide a translation. 

### Assumed Requirements

These are the main requirements that shaped the specific characteristics of hypathia.net:

<dl>
 <dt>The organization cannot use computing resources of a public cloud provider</dt>
 <dd>This may be due to procurement rules, legal obligations or the wish to run highly 
 sensitive workloads in-house, rather than on hardware that the organization has virtually no control over.</dd>
 <dt>The smallest workloads can be <em>very</em> small.</dt>
 <dd>While a cluster build from hypathia.net can run applications that span hundreds of nodes, it will also efficiently handle minimalistic experiments that consist of a single web-page, requested only a handful of times by a single user.</dd>
 <dt>Users cannot be assumed to be able to run their own cluster</dt>
 <dd>Actually running a Kubernetes cluster takes quite some expertise and time. Requiring every user to do this is not only inefficient with regard to the time of the user, but also makes efficient sharing of computing resources much more complex than need be. Especially when ombined with the previous requirement of very small workloads.</dd>
 <dt>Minimal support resources available </dt>
 <dd>All functions of the cluster must be made available to the users via self-service web interfaces, or be completely automated. Users should also be able to help themselves or each other, so tools for building a vibrant community must also be supported.</dd>
</dl>

### Aims

There are a number of further goals that the design of the cluster strives to support:

* **Secure**. This covers all aspects, from isolating one workload from the next, to ensuring that the cluster is sufficiently hardened against external and internal denial-of-service attacks. All communication should be done via encrypted and mutually authenticated TLS links.
* **Stable**. Resource scarcity may not be the result of an intentional attack, but rather the result of a massive load test by a research group. Sufficient limits and quotas must ensure that all users can work in parallel without interruption. Scarce resources must be scheduled in a transparent manner that is understood by the parties competing for use.
* **Use of GitOps principles** The configuration of the cluster must be the result of a versioned  set of instructions that can be compared and distributed between hypathia.net installation. No pets allowed inside.

<img style="width: 8em; display: block; margin-left: auto; margin-right: auto;" alt="No Pets allowed" src="/img/docs/introduction/no_pets_allowed.png"/>


### Origins

The documentation on this website was extended and restructured from the fantastic Kubernetes tutorial "Kubernetes the Hard Way"(https://github.com/kelseyhightower/kubernetes-the-hard-way) by Kelsey Hightower. Parts are reproduced by his kind permission.

The prototype cluster, that was created by working through his tutorial was then turned into a production system over time. A snapshot can be seen in  [Kubernetes the HAW way](https://github.com/christianhuening/kubernetes-the-haw-hamburg-way). From here the documentation was generalized and restructured. The [Informatik Compute Cloud](https://icc.informatik.haw-hamburg.de) remains a reference implementation of the Hypathia.net cluster.

