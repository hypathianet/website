+++
date = "2018-05-17T06:30:49+02:00"
weight = 1
description = "Introduction to hypathia.net including mission statement and scope of the project."
title = "Introduction"
draft = false
bref= "hypathia.net is a Kubernetes stack aimed at providing computing resources for academic institutions. This requires accepting an operating environment and forms of resource requests that differ somewhat from the more conventional deployment of a single cloud scale application."
toc = true
+++

### Mission

hypathia.net aims to provide a blueprint for operating a service to provide 
cloud based computing resources in a self-service manner. It presents an opionated
selection of components, configurations and practices tailored to environments that
have a specific set of requirements, often found at academic institutions. 


### Requirements

These are the main requirements that shaped the specific characteristics of hypathia.net:

<dl>
 <dt>The organisation cannot use computing resources of a public cloud provider</dt>
 <dd>This may be due to procurement rules, legal obligations or the wish to run highly sensitiver workloads in-house, rather than on hardware that the organisation has virtually no control over.</dd>
 <dt>The smallest workloads can be <em>very</em> small.</dt>
 <dd>While a cluster build from hypathia.net can run applications that span hundreds of nodes, it will also efficiently handle minimalistic experiments that consist of a single web-page, requested by a single user.</dd>
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

<center>![No Pets allowed](/img/docs/introduction/no_pets_allowed.png "No Pets allowed")</center>

### Resultant Architecture

* Gitlab as main user interface for group management and version control, collaboration and communication.
* Kubernetes in a multi-tenant configuration for orchestrating containers based applications
* Container Linux as operating system for nodes.
* Ceph as storage solution
* LDAP for password and identity management.
