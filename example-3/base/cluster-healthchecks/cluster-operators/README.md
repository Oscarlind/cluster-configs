# Checking healh of cluster operators in openshift  

## Link of interest 
## Useful commands

```bash
(SBX) root@de08-la010 ~ # oc get clusteroperators.config.openshift.io
NAME                                       VERSION   AVAILABLE   PROGRESSING   DEGRADED   SINCE   MESSAGE
authentication                             4.12.15   True        False         False      104m
baremetal                                  4.12.15   True        False         False      40d
cloud-controller-manager                   4.12.15   True        False         False      40d
cloud-credential                           4.12.15   True        False         False      40d
cluster-autoscaler                         4.12.15   True        False         False      40d
config-operator                            4.12.15   True        False         False      40d
console                                    4.12.15   True        False         False      5h20m
control-plane-machine-set                  4.12.15   True        False         False      40d
csi-snapshot-controller                    4.12.15   True        False         False      40d
dns                                        4.12.15   True        False         False      40d
etcd                                       4.12.15   True        False         False      40d
image-registry                             4.12.15   True        False         False      3h2m
ingress                                    4.12.15   True        False         False      5h18m
insights                                   4.12.15   True        False         False      40d
kube-apiserver                             4.12.15   True        False         False      40d
kube-controller-manager                    4.12.15   True        False         False      40d
kube-scheduler                             4.12.15   True        False         False      40d
kube-storage-version-migrator              4.12.15   True        False         False      39d
machine-api                                4.12.15   True        False         False      40d
machine-approver                           4.12.15   True        False         False      40d
machine-config                             4.12.15   True        False         False      6d4h
marketplace                                4.12.15   True        False         False      40d
monitoring                                 4.12.15   True        False         False      3d22h
network                                    4.12.15   True        False         False      40d
node-tuning                                4.12.15   True        False         False      40d
openshift-apiserver                        4.12.15   True        False         False      3d11h
openshift-controller-manager               4.12.15   True        False         False      40d
openshift-samples                          4.12.15   True        False         False      40d
operator-lifecycle-manager                 4.12.15   True        False         False      40d
operator-lifecycle-manager-catalog         4.12.15   True        False         False      40d
operator-lifecycle-manager-packageserver   4.12.15   True        False         False      40d
service-ca                                 4.12.15   True        False         False      40d
storage                                    4.12.15   True        False         False      17d

```
