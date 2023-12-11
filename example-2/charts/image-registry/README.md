# Image registry
This Helm chart manages the image registry (`configs.imageregistry.operator.openshift.io`) in the openshift-image-registry _project_.
The `Image Registry Operator` runs in the openshift-image-registry _namespace_, and manages the registry instance in that location as well. All configuration and workload resources for the registry reside in that namespace.

On platforms that do not provide shareable object storage, the OpenShift Image Registry Operator bootstraps itself as Removed. This allows openshift-installer to complete installations on these platform types.
After installation, you must edit the Image Registry Operator configuration to switch the managementState from Removed to Managed. This is done here in this chart together with configuring it to run on infra nodes and use persistent storage.

It will set it up to use Swift object storage.

Use the cluster specific values.yaml files to enable/disable the managed state of the registry and also handle the credentials.
## Configuration

The customizable values are:
- Whether the image registry should be persistent or not.
- Credentials for it to use Swift object storage.
- Whether the registry should run on infra nodes or not.

Example of all configurables:
```yaml
imageRegistry:
  persistent: true
  infraNode: true
  replicas: 1
  password: ""
  username: ""
  container: "" 
  tenant: ""
```
## About the image registry
Further reading can be done using the official documentation.

[OpenShift documentation](https://docs.openshift.com/container-platform/latest/registry/configuring-registry-operator.html)
