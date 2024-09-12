# Cluster Health Checks
This component holds the different cluster health checks.

## Health Checks

| Health Check                                                 | Description                                    |
| ------------------------------------------------------------ | ---------------------------------------------- |
| cluster-operators                                            | Checking for non-ready Cluster Operators       |
| kubeadmin-user                                               | Checking if the kubeadmin user exists          |
| openshift-certs                                              | Checking for expiring system certificates      |
| pods                                                         | Checking for pods in terminating state         |
| terminating-namespace                                        | Checking for terminating namespaces            |
