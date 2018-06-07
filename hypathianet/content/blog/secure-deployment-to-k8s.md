+++
title = "More Deployment Mojo"
date = "2018-05-17T11:45:40+02:00"
tags = ["kubernetes authz"]
categories = ["Deployment"]
banner = "img/blog/sicheres-deployment-nach-k8s/ali-morshedlou-590863-unsplash.jpg"
author = "Lutz Behnke"

draft = false
weight = 180
description = "Secure deployment from the build pipeline into the cluster (XLATE)"
bref = "Secure deployment from the build pipeline"

+++

## Deploying into the ICC

Let's assume the build pipeline of GitLab has finally succeeded and created a new container image. Naturally, now I want to have the pipeline deploy it into the cluster. Or to be more precise, to change the configuration of the cluster in such a manner, that it will pull the new image and then restart the ports according to the [update rules] (https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) of Deployments and Replica Sets.


For this I have to set only a few variables in my `.gitlab-ci.yml` and a corresponding job must be present:

```
variables:
  DOCKER_REGISTRY: "docker-hub.informatik.haw-hamburg.de"
  SERVICE_NAME: "haw-world"

deploy_image:
  environment:
    name: ICC-K8s
  stage: deploy
  image: nexus.informatik.haw-hamburg.de/kubectl:v1.10.2
  script:
  - sed "s+$DOCKER_REGISTRY/$SERVICE_NAME+$DOCKER_REGISTRY/$SERVICE_NAME:$CI_PIPELINE_ID+g" deploy.yaml >deploy_new.yaml
  - kubectl apply -f deploy_new.yaml
```


More information can be found in the `userdoc` section [Deploying an application from GitLab to the ICC] (https://userdoc.informatik.haw-hamburg.de/doku.php?id=docu:informatikcomputecloud#deployment_einer_applikation_aus_gitlab_in_die_icc).

### Everything insecure?

Can every user in every namespace in the ICC cluster make changes? No, only those users who have corresponding rights in the Gitlab for the corresponding groups or projects (* Owner *, * Master * to change, * Developer * to request information).

If I try to do the same things as in the CI script from my local machine, it will fail and I will point out that I am not authorized to make the change to the deployment.

Only when I authenticate against the cluster with the help of `kubelogin` will the change be accepted (for namespaces where I am at least * master *).

## Behind the scenes: more admin-mojo

For the Gitlab-Runner to be able to make changes for all namespaces, a special integration between Gitlab and Kubernetes is necessary.

Luckily, Gitlab supports this integration. (Details in the [Gitlab Documentation] (https://docs.gitlab.com/ee/user/project/integrations/kubernetes.html)) and even expanding it continuously (see below). Unfortunately, GitLab makes the assumption that every user has the opportunity to create their own clusters at will, e.g. by launching virtual machines with cloud providers like AWS or GKE. In the HAW this is not possible. Therefore, the ICC uses a different model where all users work in their own namespaces in a shared cluster.

There is a service in the ICC called 'gl-k8s-integrator`. This ensures that there is the following for each namespace in the Kubernetes cluster:

* A `ServiceAccount` named` gitlab-serviceaccount` which is asserted with the role `gitlab-group-master`. This scooter allows you to read, delete, and modify all objects in a namespace, as well as create new objects in the namespace.
* A `secret` containing a token.

In addition, in every project at Gitlab, it incorporates the following into Kubernetes integration:

* The URL under which GitLab can reach the Kubernetes API.
* The CA certificate for Gitlab to securely communicate with the Kubernetes API through TLS.
* The namespace corresponding to the project.
* The token from the `Secret` mentioned above.

If the following fragment is found in a job of the CI / CD script:
```
  environment:
    name: ICC-K8s
```

then GitLab sets automatically the correct settings, so that `kubectl` landed not only in the correct namespace, but also has the necessary rights to make changes there.

## Nachtrag

As can be seen in Gitlab's documentation, the Kubernetes integration has been deprecated in its current form. We will adjust the current functionality in a timely manner with the current implementation of the Kubernetes connection. The ICC model to use an existing, shared cluster is already provided for by Gitlab's documentation (https://docs.gitlab.com/ee/user/project/clusters/index.html#adding-an-existing- kubernetes-cluster).

## Colophon

Photo by Ali Morshedlou on Unsplash
