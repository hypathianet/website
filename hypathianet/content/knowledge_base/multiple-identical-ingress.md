

# Symptoms

All paths of an exported resources will show `503 Service Temporarily Unavailable` for all paths.

# Cause

multiple ingress objects expose the same hostname

# How to find it

if `site.example.com` is the exposed name:

```~$  kubectl get ingress --all-namespaces -o wide | grep site.example.com
namespace1                name-ingress         site.example.com              80, 443   4d
mynamespace               my-ingress           site.example.com              80, 443   5d
production                prod-ingress         site.example.com              80, 443   18h```

