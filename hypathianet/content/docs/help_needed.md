+++
title = "Help Needed"
description = "There are a number of areas of the cluster that need attention"
bref= ""
date = "2018-05-18T11:15:43+02:00"
draft = false
toc = true
+++

Currently the are a number of holes in the automation of the hypathia system. It should be possible to close them, with the construction of a number of operators and controllers for the kubernetes system. An overview can be seen in this graphic: 
{{< figure src="/img/docs/help_needed/Kubernetes_Tenant_Ecosystem.svg" title="Kubernetes Tenant Ecosystem" width="50%">}}


### OwnerListAdmissionController

In order to use tools like [Draft](https://azure.microsoft.com/en-us/blog/streamlining-kubernetes-development-with-draft/), users have to be able to list namespaces. But as granting this right would result in exposing the information about other users namespaces, this is not permitted in the current settings.

The OwnerListAdmissionController must retrieve the information to which namespaces the calling account has access to from either the GitLab directly or better yet from a ownership service running within the cluster. It then must filter all requests for namespaces to only return those that the user has permission to read from.


### CleanupOperator

### TenantOperator

### TenantQuotaAdmissionController

### TamiasUI

### DNSIngressController

### HAPIngressController

