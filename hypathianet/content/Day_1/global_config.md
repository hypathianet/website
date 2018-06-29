+++
title = "Global Configuration"
description = "Collect all the needed information before starting with the cluster setup."
weight = 100
draft = false
+++

When preparing the setup of a hypathia.net cluster, you will need to collect a few pieces information. You should prepare a shell script with these variables as defined below, so you can set them before generating configuration files. The names of the config details will be used in all further configuration templates shown on this website.

You must source the configuration file prior to the execution of any of the shell commands during the installation.

The examples below are created for a domain `example.com` and the machines in the cluster are all in the domain `cluster.example.com`. Adjust to your locale.

```
me@mynode:~$ source global_config.sh
```

# Global Certificate Information

```
## CA certificate issuer Information
CERT_CA_C="DE"
CERT_CA_L="Hamburg"
CERT_CA_O="My Fancy Cluster"
CERT_CA_ST="Hamburg"
```

# Network

For the creation of the `etcd`, which acts as the persistent store of the cluster, you will need 5 Machines and a IP address for each. The etcd can either be run on separate physical or virtual machines separate to the cluster. Alternatively a selected set of worker nodes can be choosen.

The IP addresses must be reachable from all worker and master nodes, but do not *need* to be public routable (The example values are not). They must not be from the `10.0.0.0/8` address space.

```
## IP Addresses of the etcd nodes (space separated IP addrs).
ETCD_NODES_IP_ADDRS="192.168.100.1 192.168.100.2 192.168.100.3 192.168.100.4 192.168.100.5 "
```


A number of nodes must be assigned as master or control nodes. If the cluster has not much more than a dozen nodes, each worker node may also be a master node. There must be at least three master servers for high availability reasons.
(TODO: verify this).

These IP addresses must be reachable from all worker and other master nodes. They must be able to reach the etcd nodes. They do not need to be publicly routable.  They must not be from the `10.0.0.0/8` address space.

```
## Control Node IPs (space separated IP addrs)
K8S_CONTROL_NODE_IP_ADDRS="192.168.101.1 192.168.101.2 192.168.101.3
```

Nodes must be given valid DNS names that can be looked up by all components of the cluster. They should be FQDNs.

```
##  Control Node Names (space separated FQDNs)
K8S_CONTROL_NODE_NAMES="ctrl1.cluster.example.com ctrl2.cluster.example.com ctrl3.cluster.example.com"
```

A front end node is the single access point for the cluster. It will carry all API traffic as well as the user traffic entering and exiting the cluster. For the intended type and size of clusters, this should be sufficient as the traffic, provided it is carried over multiple 10G ethernet links, bonded to a single logical interface.

For this front end the IP address and the FQDN name are needed. The IP address must be public routable and should be reachable from all control and worker nodes.

```
## Front end IP addr
K8S_FRONTEND_IP_ADDRS="141.22.10.222"
```

```
## Front end node name
K8S_FRONTEND_NAME="cluster-k8s-api.cluster.example.com"
```

# GitLab
## Web Application

The public hostname of the GitLab instllation

```
GITLAB_EXTERNAL_HOST="gitlab.example.com"
```

## Database

This the password for the GitLab database. Ideally you will create it with
```
me@mynode:~$ date | sha256sum -
```

```
GITLAB_DB_PASSWORD="a-very-fine-and-really-long-secret"
```

