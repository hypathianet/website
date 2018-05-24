+++
title = "Mehr Deployment Mojo"
date = "2018-05-17T11:45:40+02:00"
tags = ["kubernetes authz"]
categories = ["Deployment"]
banner = "img/blog/sicheres-deployment-nach-k8s/ali-morshedlou-590863-unsplash.jpg"
author = "Lutz Behnke"

draft = false
weight = 180
description = "Sicheres Deployment von der Build Pipeline in den Cluster"
bref = "Sicheres Deployment aus der Build Pipeline"

+++

## Deploying in die ICC

Angenommen die Build-Pipeline von Gitlab war endlich erfolgreich und hat ein neues Container Image erstellt. Nun möchte ich es natürlich auch von der Pipeline im Cluster deployen lassen. Oder um genauer zu sein, die Konfiguration des clusters derart zu ändern, das er das neue Image lädt und dann gemäß der [Update Regeln](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment) von Deployments und Replica Sets die Pods neustartet.

Dazu muss ich in meiner `.gitlab-ci.yml` nur ein paar Variablen setzen und ein entsprechender Job enthalten sein:

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


Mehr Informationen finden sich im `userdoc` Abschnitt [Deployment einer Applikation aus GitLab in die ICC](https://userdoc.informatik.haw-hamburg.de/doku.php?id=docu:informatikcomputecloud#deployment_einer_applikation_aus_gitlab_in_die_icc).

### Alles unsicher?

Darf nun jeder User in jeden Namespace im ICC Cluster Änderungen durchführen? Nein, nur jene User, die im Gitlab für die entsprechenden Gruppen oder Projekte entsprechende Rechte haben (*Owner*, *Master* um ändern, *Developer* um Informationen abzufragen).

Wenn ich versuche die gleichen Schritte wie im CI Script von meinem lokalen Rechner ausführe, wird dies fehlschlagen und ich werde den Hinweis das ich nicht berechtigt bin die Änderung des Deployments durchzuführen.

Erst wenn ich mit mit Hilfe von `kubelogin` gegenüber dem Cluster authentifiziere wird die Änderung akzeptiert (für Namespaces in denen ich mindestens *Master* bin).

## Hinter den Kulissen: mehr Admin-Mojo

Damit der Gitlab-Runner für alle Namespaces Änderungen durchführen darf, ist eine spezielle Integration zwischen Gitlab und Kubernetes notwendig.

Zum Glück unterstützt Gitlab diese Integration. (Details in der [Gitlab-Dokumentation](https://docs.gitlab.com/ee/user/project/integrations/kubernetes.html)) und baut diese sogar kontinuierlich aus (siehe unten). Leider macht GitLab die Annahme das jeder Benutzer die Möglichkeit hat, nach Belieben eigene Cluster anzulegen, z.B. durch das Starten von virtuellen Maschinen bei Cloud Anbietern wie AWS oder GKE. In der HAW ist dies nicht möglich. Daher verwendet die ICC ein anderes Modell bei dem allen Benutzer in eigenen Namespaces in einem gemeinsam verwendeten Cluster arbeiten.

Dazu gibt es in der ICC einen Dienst namens `gl-k8s-integrator`. Dieser stellt sicher, das es für jeden Namenspace im Kubernetes Cluster folgende Dinge gibt:

* Einen `ServiceAccount` namens `gitlab-serviceaccount` der mit der Rolle `gitlab-group-master` assiziert ist. Diese Roller gestattet es alle Objekte in einem Namespace zu lesen, löschen und zu ändern, sowie neue Objekte in dem Namespace anzulegen.
* Ein `Secret` das einen Token enthält.

Außerdem trägt es in jedem Projekt im Gitlab folgende Dinge in die Kubernetes Integration ein:

* Die URL unter der GitLab die Kubernetes API erreichen kann.
* Das CA Zertifikat, damit Gitlab sicher über TLS mit der Kubernetes API kommunizieren kann.
* Den Namespace der dem Projekt entspricht.
* Den Token aus dem `Secret`, das oben erwähnt wurde.

Wenn nun in einem Job des CI/CD Scripts folgendes Fragment steht:

```
  environment:
    name: ICC-K8s
```

dann setzt GitLab automatisch die richtigen Einstellungen, so das mit `kubectl` nicht nur im richtigen Namespace landed, sondern auch die notwendigen Rechte hat um dort Änderungen auszuführen.

## Nachtrag

Wie in der Dokumentation von Gitlab zu sehen ist, wurde die Kubernetes Integration in der momentanen Form als veraltet markiert ("deprecated"). Wir werden die momentane Funktionionalität zeitnah mit der aktuellen Umsetzung der Kubernetes-Anbindung nachstellen. Das ICC Modell einen existierenden, gemeinsam verwendeten Cluster zu verwenden ist der Dokumentation von Gitlab sogar schon [vorgesehen](https://docs.gitlab.com/ee/user/project/clusters/index.html#adding-an-existing-kubernetes-cluster).

## Colophon

Photo by Ali Morshedlou on Unsplash
