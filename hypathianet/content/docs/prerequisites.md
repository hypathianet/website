

You will need

* a network with internet access as well as a sufficient number of available IP adresses. Be sure to allow for growth (even when successful beyond your wildest dreams!). Migrating the cluster from one network to another is possible, but not for the faint of heart.
* a DNS domain and a mechanism to integrate it with your super-domain (eg. compute-cloud.example.com).
* entries for all your users in an Directory System that allows access via LDAP. Each user entry must have at least the user name, the full name, the email-address and the password of the user (the passwort not directly readable, but in such a way that it enables the bind operation with the LDAP server).
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

