# Pods

Health check looking for unhealthy pods in a cluster. This is useful as it will present a visual cue directly in the ACM Governance section.


## Useful commands

To check a similar result on demand (single cluster), run the following
```bash
oc get pods --all-namespaces | grep -v Running
```
This returns all pods excluding non-running ones.
