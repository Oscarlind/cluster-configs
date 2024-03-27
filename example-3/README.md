# Cluster-configuration
This repository contains
* cluster-configuration in the form of Kustomize manifests.
* Representations of OpenShift environments in the form of subdirectories in overlays/$cluster

## Overview
The repository structure looks like this. Under **base** we have the cluster configuration, such as image-registry, logging stack, monitoring etc. templated as Kustomize manifests.

In the **overlay** directory we have sub-directories for each cluster, e.g. **dev, prod** etc. 

For each cluster in **overlays/$CLUSTER** such as **overlays/dev** an [App Of Apps Pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern) is being used for the cluster-configuration.

```bash
├── base                   # 1
│   └── pacman
│       └── resources
└── overlays               # 2
    ├── dev
    │   └── pacman
    |       └── appliaction.yaml   #3
    │       └── resources
    ├── non-prod
    ├── prod
    └── sandbox
```
In this structure, we add [ArgoCD Applications](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#applications) under the **Cluster Name** that we want to deploy to. For example *dev*. This ArgoCD application will then point to an [overlay](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/#bases-and-overlays) that uses the **base** directory on the top level.

1. Location for our cluster configuration
2. Directory containing our clusters with specific patchs
3. appofapp patterns 

Each cluster would get ArgoCD installed with a pointer to the specific directory in this tree above, that matches the requirements. For example, **dev** on **overlays** would have have the pointer:

```yaml
  source:
    path: "example-3/overlays/dev"
    repoURL: "https://git@github.com/oscarlind/cluster-configs
```
## Usage

ArgoCD consumes this repository pointing at the cluster specific subdirectories depending on the cluster.
That directory can be populated with helm charts and other yaml content.

### Add new cluster configuration
Each cluster has a directory:

- [dev](./overlays/dev/)
- [non-prod](./overlays/non-prod/)
- [prod](./overlays/prod/)

1. Add Kubernetes manifests inside a new folder, for example base/my-new-configuration.
2. Create an overlay with the required customizations, for example overlays/my-new-configuration.
3. Add an Argo Application in the specific directory, for example in overlays/dev, pointing to the specific overlay:

    ```yaml
    apiVersion: argoproj.io/v1alpha1
    kind: Application
    metadata:
        name: my-new-configuration
        namespace: platform-gitops                                                       # < Namespace for the Argo application (1)
    spec:
        project: default
        source:
            repoURL: ssh://git@github.com/oscarlind/cluster-configs.git                  # < The configuration repository (2)
            targetRevision: master                                                       # < Branch - main or master      (3)
            path: overlays/dev/my-new-configuration                                      # < Path to our customization    (4)
        destination:
            server: https://kubernetes.default.svc
            namespace: my-new-namespace                                                  # < Namespace to deploy the manifests to (5)
    ```
    1. This is the namespace the ArgoCD application itself will be deployed to. This is **NOT** the actual content of the manifests
    2. Point to the repository containing the cluster-configuration. This should be the overlay of the manifests that we want to deploy
    3. The branch that is being used. Usually "Main" or "Master" when using common and simple branching strategies for GitOps [Read more](https://github.com/oscarlind/cluster-configs#why-not-environment-branches)
    4. This is the overlay we will use, containing the cluster-configuration that should be deployed. Can be, LDAP, Monitoring, Logging etc. Each component has it's own chart.
    5. Namespace where the manifests will be deployed to.



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
