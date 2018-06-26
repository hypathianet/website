+++
title = "Permissions"
description = "Controlling access permissions of users and components. (Docs WIP)"
bref= "User permissions are managed by allowing users to organize themselves in Gitlab and assigning permissions appropriately. Multiple technical protocols enforce these permissions throughout the cluster."
date = "2018-05-17T06:30:49+02:00"
toc = true
weight = 700
draft = false
+++

### User Groups

User organize themself by creating projects and groups in Gitlab

#### Namespace Mapping

### Cluster Authentication

#### LDAP Authentication

#### kubelogin

### RBAC

### Service Accounts

### Capabilities

The required capabilities for Conduit (a PSP with CAP_NET_ADM activated) are made availabe via a special service account `conduit`. Deployments that wish to use the Service Mesh must assign the service account in der deployment description. 
