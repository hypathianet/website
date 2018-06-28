+++
date = "2018-06-27T11:40:18+02:00"
title = "CNI Plugin not found"
description = "Hyperkube image loaded from gcr.io instead of quay.io will fail to load CNI plugin"
weight = 100
draft = false
+++

toc = true

### Symptoms

Hyperkube image loaded from gcr.io instead of quay.io will fail to load CNI plugin with the log message
```
Error adding network: failed to find plugin "calico" in path [/opt/calico/bin /opt/cni/bin]
```

### Cause

Other than the image provided CoreOs, the one provided by Google does not contain the CNI plugins. They need to be loaded from the host disk and the necessary directory need to be mapped as devices into the kubelet container

### Solution

Add
```
--volume opt-cni-bin,kind=host,source=/opt/cni/bin     --mount volume=opt-cni-bin,target=/opt/cni/bin
```

to the RKT_RUN_ARGS environment variable. 
 
