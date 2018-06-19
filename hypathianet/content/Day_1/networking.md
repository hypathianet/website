+++
date = "2018-05-31T06:12:26+02:00"
weight = 3
description = "Networking, including name resolution via DNS and IP Address Management (IAM)"
title = "Introduction"
draft = false
bref= "Network management in an Kubernetes cluster encompasses two vital areas: mangement of the node network, from the physical medium up to the addresses of the nodes of the cluster. And the network topology and infrastructure to connect the pods and containers running within the cluster."
toc = true
+++


### Node Network

### Pod Network

#### Automatic Certificate Generation

### Frontend / Load Balancer

#### Make Service available to the World

* add DNS resolution for name of the service. It should resolve to the IP Address of the frontend node.
* add line to `/etc/haproxy/sni-domains.lst` on the frontend node.
