+++
title = "Help Needed"
description = "There are a number of areas of the cluster that need attention"
bref= ""
date = "2018-05-18T11:15:43+02:00"
draft = false
toc = true
+++

Currently, the are a number of holes in the automation of the hypathia system.
There are also parts that are unnecessarily complex. Most of these are the result of the organic growth of the system from it's original incubation invironment.

## Infrastructure

### Deploy Configuration Management System

Hypathia.net has grown out of an environment that uses [Puppet](https://puppet.com/#) for configuration management. But this requires further infrastructure that is not worthwhile for setting up the few pet nodes that make up the Hypathia.net bootstrap infrastructure.

Terraform is used to set up the configuration of the Matchbox server, but not utilized for the other machines.

Any new (or old) system should allow the step-by-step installation of all components of the system, be easily readable by humans and not require an additional server/infrastructure setup.

### Unify DHCP Server

Currently there are separate DHCP servers for distributing IP addresses and for injecting node identity and configuration information. The reason for this is the lack of LDAP backend support of the matchbox server.

A solution to merge the configuration data to a single source of truth is needed.

## Cluster Components

It should be possible to close them, with the construction of a number of operators and controllers for the kubernetes system. An overview can be seen in this graphic:
{{< figure src="/img/docs/help_needed/Kubernetes_Tenant_Ecosystem.svg" title="Kubernetes Tenant Ecosystem" width="50%">}}


### OwnerListAdmissionController

In order to use tools like [Draft](https://azure.microsoft.com/en-us/blog/streamlining-kubernetes-development-with-draft/), users have to be able to list namespaces. But as granting this right would result in exposing the information about other users namespaces, this is not permitted in the current settings.

The OwnerListAdmissionController must retrieve the information to which namespaces the calling account has access to from either the GitLab directly or better yet from a ownership service running within the cluster. It then must filter all requests for namespaces to only return those that the user has permission to read from.

(work in progress: will ask K8s API for information on namespaces and their rolebinding)

### CleanupOperator
(work in progress)

### TenantOperator

(work in progress: read Tenant CRD, create/maintain namespaces, RoleBinding)


### TenantQuotaAdmissionController

### TamiasUI
(work in progress)

### DNSIngressController
(aka CName-Svc)



### HAPIngressController

