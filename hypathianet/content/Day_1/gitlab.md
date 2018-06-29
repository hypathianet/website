+++
title = "GitLab"
description = "Installation of GitLab"
draft= false
weight = 400
+++

## Create a GitLab Instance

1. Install three machines (virtual or physical) with the current Ubuntu LTS with the following minimal requirements in the following order:
  1. The PostgreSQL Database machine: 2 Cores, 8GB RAM, 100GB system and database disk.
  1. The GitLab Web Application: 2 cores, 8GB RAM, 16GB system disk, sufficient storage for user data. 1TB per 1000 Users as a very rough estimate.
  1. The GitLab Runner: 2 Cores, 2GB RAM, 16 GB system disk


more details can be found on the [GitLab installation page](https://about.gitlab.com/installation/#ubuntu)

### Postgresql Database server

You could install the PostgreSQL Database manually, but using the GitLab Omnibus distribution make this much easier.

```
#
# set up database via the gitlab configuraion method

external_url 'https://${GITLAB_EXTERNAL_HOST}'

# Disable all components except PostgreSQL
postgresql['enable'] = true
bootstrap['enable'] = false
nginx['enable'] = false
unicorn['enable'] = false
sidekiq['enable'] = false
redis['enable'] = false
gitlab_workhorse['enable'] = false
mailroom['enable'] = false

# PostgreSQL configuration
gitlab_rails['db_password'] = '${GITLAB_DB_PASSWORD}'
gitlab_rails['auto_migrate'] = false

postgresql['md5_auth_cidr_addresses'] = ['0.0.0.0/0']
postgresql['listen_address'] = '0.0.0.0'

```

### Web Application

### Background Information

The separation of the GitLab web application from the database allows a stepwise extension of the installation to support growing user demands. The above values are sufficient to support about a 1000 users in an academic environment (students and faculty). Depending on the type of organization this may vary widely.

The setup documented here is a minimal variant of the [HA installation](https://docs.gitlab.com/ee/administration/high_availability/README.html) documented by GitLab. It is not the fully fledged HA and scale-out version. Rather it is a more reduced one that is future proof, but avoids the high overhead as long there is no initial need for it.

GitLab also offers a variant of the system to run inside the cluster itself with the help of a prepared [Helm chart](https://docs.gitlab.com/ee/install/kubernetes/). But that adds the problem of a chicken-and-egg problem if either should fail and the separate setup avoids this complexity. The Hypathia.net design contains of two pets that contain the source of truth and help manage the whole system: the etcd database and the GitLab installation.

### Support and GitLab Version

This documentation assumes that the community version of GitLab is used in setting up the cluster environment. It is available as open source and licensed under the MIT License.

For most academic organizations, GitLab offers also the much extended Ultimate Version free of charge, but without support.

There are also support contracts available with academic pricing.