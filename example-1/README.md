# Cluster-configuration
This repository contains
* cluster-configuration in the form of Kustomize manifests.
* Representations of OpenShift environments in the form of subdirectories in clusters/$cluster

## Overview
The repository structure looks like this. Under **base** we have the cluster configuration, such as image-registry, logging stack, monitoring etc. templated as Kustomize manifests.

In the **clusters** directory we have sub-directories for each cluster, e.g. **dev, prod** etc. 

For each cluster in **clusters/$cluster** such as **clusters/dev** an [App Of Apps Pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) is being used for the cluster-configuration.

```bash
├── base                   # 1
│   └── pacman
│       └── resources
├── clusters               # 2
│   ├── dev
│   ├── non-prod           # 3
│   ├── prod
│   └── sandbox
```
In this structure, we add [ArgoCD Applications](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications) under the **Cluster Name** that we want to deploy to. For example *dev*. This ArgoCD application will then point to an [overlay](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/#bases-and-overlays) that uses the **base** directory on the top level.

1. Location for our cluster configuration
2. Directory containing our clusters
3. Sub-directory for a cluster - in this case the cluster specific overlay

Each cluster would get ArgoCD installed with a pointer to the specific directory in this tree above, that matches the requirements. For example, **dev** on **clusters** would have have the pointer:

```yaml
  source:
    path: "example-1/clusters/dev/appofapps"
    repoURL: "https://git@github.com/oscarlind/cluster-configs
```
## Usage

ArgoCD consumes this repository pointing at a **appofapp** helm chart, that is specific for the cluster.
That directory can be populated with overlays which then are referred in the appofapp specific value file.

### Add new cluster configuration
Each cluster has a directory:

- [dev](./clusters/dev/)
- [non-prod](./clusters/non-prod/)
- [prod](./clusters/prod/)

1. Add Kubernetes manifests inside a new folder, for example base/my-new-configuration.
2. Create an overlay with the required customizations, for example clusters/dev/my-new-configuration.
3. Add an Argo Application by using the appofapp helm chart, pointing to the specific overlay:

    ```yaml
    cluster-proxy:                                             # 1
      annotations:
        argocd.argoproj.io/sync-wave: "11"                     # 2
        argocd.argoproj.io/sync-options: ServerSideApply=true  # 3
      source:
        path: clusters/dev/cluster-proxy                       # 4
    ```
    1. The name of the ArgoCD application that will be created.
    2. Sync order of the application (lower gets synced first)
    3. Sync setting
    4. This is the overlay we will use, containing the cluster-configuration that should be deployed. Can be, LDAP, Monitoring, Logging etc.

    Documentation: https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications

3. Create branch with Jira case number.

    ```sh
    git checkout -b JIRA-123_short_description
    ```

4. Commit with Jira case number in message.

    ```sh
    git add .
    git commit --message "JIRA-123 added ..."
    ```

5. Push.

    ```sh
    git push
    # if it fails, use this configuration
    # git config --global push.default current
    ```

6. Create pull request in.
7. Get approval from reviewer in pull request.
8. Merge to master.
9. _platform-gitops_ ArgoCD will apply changes to cluster.
