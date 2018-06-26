+++
date = "2018-05-30T16:29:59+02:00"
weight = 2
description = "Technical prerequisites and recommended hardware"
title = "Prerequisites"
draft = false
bref= "While hypathia.net aims to be as self contained as possible, there are some infrastructure elements that need to be provided. While a test installation is possible on virtual machines, a production environment is targeted to run on dedicated hardware. This section includes a purchase recommendation and the information that motivated it."
toc = true
+++

## Infrastructure Environment

You will need

* a network with Internet access as well as a sufficient number of available IP addresses. Be sure to allow for growth (even when successful beyond your wildest dreams!). Migrating the cluster from one network to another is possible, but not for the faint of heart.
* a DNS domain and a mechanism to integrate it with your super-domain (eg. compute-cloud.example.com).
* entries for all your users in an Directory System that allows access via LDAP. Each user entry must contain at least the user name, the full name, the email-address and the password of the user (the password not directly readable, but in such a way that it enables the `bind` operation with the LDAP server).
* Either a sufficient number of physical hardware machines or the ability to create virtual machines of the required size. Also note, that the resource requirements of these machines will vary widely. Not all need to be expensive high-power systems. But some have specific requirements that will influence their performance in specific ways. E.g. the etcd nodes need only 1-2 cores, and 8 Gig of RAM, but are extremely sensitive to IO latency.

To bootstrap the Kubernetes cluster a number of computer nodes are needed. Some may be incorporated into the cluster later on, but it they can also run as external machines continuously (e.g. if they are shared with the general IT resources of your organization or department)

* DNS and DHCP (incl. a dedicated LDAP server for use by the DNS server), 1 machine
* Matchbox DHCP server 1 machine
* HA proxy. 1 machine.
* Gitlab web app, database and ci-runner. 3 machines
* etcd. 5 machines
* bootstrap Kubernetes API servers. at least 3 machines

The following Packages will provide the infrastructure:

* PowerDNS
* Matchbox
* HA Proxy


## Hardware Recomendation

While the specific hardware for a hypathia.net cluster has to be a decision, each organization needs to make on their own. Below is a list of specific machines that represent the current optimum available for purchase. This list may change. Below the list, you will find more details on what influences shaped the selection, so that you may adapt it to your needs more easily.

Generally the machines are selected to fulfill the following aims:

* Server-class hardware with as few vendor specific features as possible. [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface) for management is a public standard and is absolutely sufficient for the intended purpose of the system.
* Low space requirements and sensible power requirements. While normally rack space costs money and is an axis of optimization, rack space in an on-prem "datacenters" of many universities is even more limited and so is available power, especially managed and backed up by an Uninteruptable Power Supply (UPS), as well as the required cooling to remove the energy after use.

### Machines

#### Network

You will need at least one switch that supports two 10Gig Ethernet connection per hardware node of the cluster. This will be the main internal network connection of the nodes of the cluster

Examples of such switches are:

* Cisco Nexus 5548UP-FA


Additionally you will need a sufficient number of 1Gig Ethernet ports for the management networks of the nodes. These should either be in a separate VLAN or be physically separate from the data network above, in order to securely manage the servers as well as distribute secrets when booting machines.


#### Compute Node

This solutions provides the highest density (4 Nodes per 2 u height), while avoiding highly proprietary blade formats or very costly extra-slim or high end mobile components.

The only exception to this is the additional network card, that is proprietory to SuperMicro chassis and motherboards.

Pos      | Description                                      | #
---------|--------------------------------------------------|-------:
**Chassis**  | SuperMicro SuperServer 6029TP-HTR TwinPro2       |  1
**CPU** | Intel® Xeon® Gold 5115 Processor 10-core 2.40GHz 13.75MB Cache (85W) | 8
**RAM**      | 16GB PC4-21300 2666MHz DDR4 ECC Registered DIMM  | 48
**SSD**      | NVMe Samsung PM1725a 1.6 TB PCIe                 | 4
**HDD**      | 1.0TB SATA 6.0GB/s 7200RPM - 3.5" - Seagate Enterprise Capacity v5 (512n)  | 4
**I/O Module** | Supermicro SIOM 1-Gigabit Ethernet Adapter AOC-MGP-i2M (2x RJ45)  | 4
**Network Card** | Intel® 10-Gigabit Ethernet Converged Network Adapter X520-SR2 (2x LC) | 4

#### Storage Node

You will need these in sets of three to provide sufficient redundancy. The parts list below is for a single system.

The Samsung M.2 Device acts as a persistent store for the CEPH monitor Data, while the NVMe SSD is used as Write-Cache for CEPH.

Pos | Description | #
----|-------------|---:
**Chassis** | SuperMicro SuperServer 6019P-WT8 | 1
**CPU** | Intel® Xeon® Gold 5115 Processor 10-core 2.40GHz 13.75MB Cache (85W) 2
**RAM** | 8GB PC4-21300 2666MHz DDR4 ECC Registered DIMM 12
**HDD** | 4.0TB SATA 6.0GB/s 7200RPM - 3.5" - Seagate Enterprise Capacity v5 (512n) | 8
**M.2** | 512 GB Samsung 960 Pro | 1
**SSD** | NVMe Enterprise AIC  Intel DC P4600  2.0TB  3 DWPD | 1
**Network Card** | Intel® 10-Gigabit Ethernet Converged Network Adapter X520-SR2 (2x LC) | 1


#### GPU Node

This is optional and only needed if you want to provide GPGPU based calculations.


Pos | Description  | #
----|--------------|---:
**Chassis** | Supermicro SuperServer 1029GQ-TRT | 1
**CPU** | Intel® Xeon® Gold 5115 Processor 10-core 2.40GHz 13.75MB Cache (85W) | 2
**GPU** | NVIDIA® Tesla™ V100 GPU Computing Accelerator - 32GB HBM2 - Passive Cooling | 4
**RAM** | 64GB PC4-21300 2666MHz DDR4 ECC Registered DIMM | 12
**SSD** | NVMe Enterprise, AIC, Intel DC P4600, 2.0TB, 3 DWPD | 1
**Network Card** | Intel® 10-Gigabit Ethernet Converged Network Adapter X520-SR2 (2x LC) | 1

### Further Reading

* Initial discussion about the purchase of server hardware for [GitLab.com Server Hardware](https://news.ycombinator.com/item?id=13153031)
* Consolidated information on a GitLab blog pos: [Proposed Server Purchase for GitLab.com](https://about.gitlab.com/2016/12/11/proposed-server-purchase-for-gitlab-com/)
