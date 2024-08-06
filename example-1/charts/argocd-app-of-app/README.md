A simple helm chart that can generate one or more Argo CD AppProject and Applications to support the App of App concept.

Use `helm package .` to package updates.

Chart adapted from example at [https://github.com/stevesea/argocd-helm-app-of-apps-example](https://github.com/stevesea/argocd-helm-app-of-apps-example)

## Using IgnoreDifferences
How to use [IgnoreDifferences](https://argo-cd.readthedocs.io/en/stable/user-guide/diffing/#application-level-configuration) via the _extraFields_ variable.

Example application in the values.yaml file with this enabled.

```yaml
    extraFields: |
      ignoreDifferences:
      - group: apps
        kind: Deployment
        jsonPointers:
        - /spec/replicas
```

## Source
* [app-of-app chart](https://github.com/gnunn-gitops/argocd-app-of-app-chart)