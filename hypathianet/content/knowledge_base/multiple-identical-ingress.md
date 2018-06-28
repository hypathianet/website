+++
date = "2018-06-27T11:40:18+02:00"
weight = 100
title = "Ingress Name Collision"
toc = true
+++


# Symptoms

All paths of an exported resources will show `503 Service Temporarily Unavailable` for all paths.

# Cause

multiple ingress objects expose the same hostname

# How to find it

if `site.example.com` is the exposed name:

```~$  kubectl get ingress --all-namespaces -o wide | grep site.example.com
namespace1                name-ingress         site.example.com              80, 443   4d
mynamespace               my-ingress           site.example.com              80, 443   5d
production                prod-ingress         site.example.com              80, 443   18h
```

# How to fix it

delete all but one of the ingresses.

