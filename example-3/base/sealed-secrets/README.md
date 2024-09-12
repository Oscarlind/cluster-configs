# Sealed-Secret
Kustomization for installing Sealed-Secrets in a cluster. This will install a controller in the cluster that will be able to decrypt the encrypted (sealed) kubernetes secrets.

## How it works
The Sealed-Secret controller will be installed in the sealed-secrets _namespace_.

This namespace along with a key pair used for encrypting and decrypting _SealedSecret_ resources will already be deployed via Ansible during the cluster installation.

Once the controller starts, it will detect the key and start unsealing (decrypting) any sealed (encrypted) secrets that exists in the cluster. It can also now be used in order to encrypt new secrets.

## How to use
Sealed-Secrets uses a binary called [kubeseal](https://github.com/bitnami-labs/sealed-secrets/pkgs/container/sealed-secrets-kubeseal). This tool is used in order to encrypt our secrets. There are two ways this can happen:

1. The _kubeseal_ binary is being run while it can reach the controller pod. This way it fetches the public key directly from the controller and uses it in order to do the encryption.
2. We have the public key saved locally and we point kubeseal to use it. With this approach one does not need to be logged in to a cluster.

If going with the first approach, we need to give the namespace of the controller as an argument. The second approach is to specify the location of the locally available key.

### Example 1.
```bash
kubeseal -o yaml --controller-namespace sealed-secrets <secret.yaml > sealed-secret.yaml
```
### Example 2.
```bash
kubeseal -o yaml --cert mycert.pem sealed-secrets <secret.yaml > sealed-secret.yaml
```
> **NOTE:** secret.yaml is the input file here, a normal k8s secret.


## Link of interest 
* [Sealed-Secret GitHub](https://github.com/bitnami-labs/sealed-secrets/)