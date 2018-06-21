

### Update CoreOS nodes

With the help of the CoreOs auto-updater. By setting an annotation on those machines that need to be updated:

```
kubectl get nodes --context admin@hypathia | grep "coreos" | awk ' //  { print $1 }' | xargs -I NODE sh -c '(kubectl annotate --context admin@hypathia --overwrite node NODE container-linux-update.v1.coreos.com/reboot-needed='true')&'

```