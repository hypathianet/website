+++
date = "2018-05-30T16:29:59+02:00"
weight = 2
description = "Technical prerequisites and recommended hardware"
title = "Prerequisites"
draft = false
bref= "While hypathia.net aims to be as self contained as possible, there are some infrastructure elements that need to be provided. While a test installation is possible on virtual machines, a production environment is targeted to run on dedicated hardware. This section includes a purchase recommendation and the information that motivated it."
toc = true
+++

### Infrastructure Environment

You will need

* a network with Internet access as well as a sufficient number of available IP addresses. Be sure to allow for growth (even when successful beyond your wildest dreams!). Migrating the cluster from one network to another is possible, but not for the faint of heart. 
* a DNS domain and a mechanism to integrate it with your super-domain (eg. compute-cloud.example.com).
* entries for all your users in an Directory System that allows access via LDAP. Each user entry must have at least the user name, the full name, the email-address and the password of the user (the password not directly readable, but in such a way that it enables the `bind` operation with the LDAP server).
* Either a sufficient number of physical hardware machines or the ability to create virtual machines of the required size. Also note, that the resource requirements of these machines will vary widely. Not all need to be expensive high-power systems. But some have specific requirements that will influence their performance in specific ways. E.g. the etcd nodes need only 1-2 cores, and 8 Gig of RAM, but are extremely sensitive to IO latency.

To bootstrap the Kubernetes Cluster a number of computer nodes are needed. Some may be incorporated into the cluster later on, but it they can also run as external machines continuously (e.g. if they are shared with the general IT resources of your organization or department)

- DNS and DHCP (incl. a dedicated LDAP server for use by the DNS server), 1 machine
- HA proxy. 1 machine.
- Gitlab web app, database and ci-runner. 3 machines
- etcd. 5 machines
- bootstrap Kubernetes API servers. at least 3 machines

The following Packages will provide the infrastructure:

* PowerDNS
* Matchbox
* HA Proxy


### Hardware Recomendation

While the specific hardware for a hypathia.net cluster has to be a decision, each organisation needs to make on their own. Below is a list of specific machines that represent the current optimum available for purchase. This list may change. Below the list, you will find more details on what influences shaped the selection, so that you may adapt it to your needs more easily.

Generally the machines are selected to fullfill the following aims:

* Server-class hardware with as few vendor specific features as possible. [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface) for management is a public standard and is absolutely sufficient for the intended purpose of the system.
* Low space requirements and sensible power requirements. While normally rack space costs money and is an axis of optimization, rack space in an on-prem "datacenters" of many universities is even more limited and so is available power, especially managed and backed up by an Uninteruptable Power Supply (UPS), as well as the required cooling to remove the energy after use.
 
#### Machines

##### Network

##### Compute Node

##### Storage Node

##### GPU Node

#### Further Reading

* Initial discussion about the purchase of server hardware for [GitLab.com Server Hardware](https://news.ycombinator.com/item?id=13153031)
* Consolidated information on a GitLab blog pos: [Proposed Server Purchase for GitLab.com](https://about.gitlab.com/2016/12/11/proposed-server-purchase-for-gitlab-com/)
