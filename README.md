# cluster-configs
Example repo showing some git structures for cluster config via GitOps. I have also included some good standards and practices.

## Repository Examples
In the **example-N** directories, I've included some examples of GitOps patterns using both Helm and Kustomize. They are opiniated and a good default baseline that I've used at multiple different customers. 

**example-1** and **example-2** are both defining the cluster by its own directory. This is populated by ArgoCD applications which makes use of either an *overlay* or a Helm chart with a cluster specific *values.yaml* file. Details are found in the respective directories README.md file.

# Standards and Good Practices when working with GitOps
When formulating standards its crucial to establish some principles and lean on them as baseline that we must follow and adhere.

- **Do** use `yaml` extention for files.
- **Do** minimize `yaml` file duplication, avoid copy and paste.
- **Do Not** deploy from environment specific branches, [read more here](#why-not-environment-branches).
- **Do** make use of PR's and peer reviewing.

## Why **Not Environment** Branches
You sometimes see organizations using permanent branches to represent different environments. You have a `dev` branch for the `dev` environment, a `test` branch for the `test` environment, etc.

This often seems like an ideal way to do things, promoting between environments simply comes a matter of merging from lower environment branches to higher environment branches. However in practice it can be quite challenging for the following reasons:

- There are often many files that are environment specific and should either not be merged between environments or need to be named uniquely to avoid collisions.
- Typically the 1:1 branch to environment works best when the manifests are identical across all branches, tools like kustomize do not fit into this pattern.
- In a microservices world, a one branch per environment will quickly lead to an explosion of branches which again becomes difficult and cumbersome to maintain over longer term.
- Difficult to have a unified view of cluster state across all environments since the state is stored in separate branches.
So in short we much prefer a single branch style with multiple folders to represent environments and clusters as we will see below.

Obviously this does not preclude using branches for updates, PRs, etc but these branches should be short lived, temporary artifacts to support development and not permanent fixtures.


## GitOps Principle & Practices and Tools

|id|  Principle      | Tool      | Capability - in the sense of a GitOps workflow                                                                                             |
|--|-----------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------|
|1 |  Declarative    | K8S/OpenShift | Express resources in a declarative manner                                                                                  |
|2|  Versioned & Immutable    | GitHub    | Stores, version controls the definitions and provide auditing over time            |
|3|  Pull Automatically    | ArgoCD    | Pull the defintions from source repositories. |
|4|  Continuously Reconciled   | ArgoCD    | It sync the desired state to the cluster.|

## Common Tools

* [Helm](https://helm.sh/)
* [Kustomize](https://kustomize.io/)

### Further reading

* [opengitops](https://opengitops.dev/)
* [What's new in Red Hat OpenShift](https://www.redhat.com/en/whats-new-red-hat-openshift)

Based on these principles, Git should be source of truth for the state of the system. Any change in configuration should occur through git which is peer reviewed, tested by colleagues and CI System.