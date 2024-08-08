# APIcast Pipeline

## DR secret configuration

### Create an access token in 3scale

1. Using the DR 3scale Admin interface, navigate to Account Settings ==> Tokens
2. Add a new token with access to all APIs and Read/Write permissions.
3. Make a note of the generated token value.

### Create the secret for APIcast configuration

1. Assume that the access token value that we just created is `7ae6d59bdf549b3f407216ea8eb42a90c28a3c1d851e11c8f30436291b7beda4`
2. From a command line, execsute the following:

```
export TOKEN=7ae6d59bdf549b3f407216ea8eb42a90c28a3c1d851e11c8f30436291b7beda4
export THREESCALE_URL=redes-admin.apps.ocp-dr.tdigital-vivo.com.br
oc create secret generic apicast-configuration-url-secret --dry-run=client --from-literal=AdminPortalURL=https://${TOKEN}@${THREESCALE_URL} --type=Opaque -n redes-3scale-apicast-dev -o yaml
```
3. The output should be similar to the following:
```
apiVersion: v1
data:
  AdminPortalURL: aHR0cHM6Ly83YWU2ZDU5YmRmNTQ5YjNmNDA3MjE2ZWE4ZWI0MmE5MGMyOGEzYzFkODUxZTExYzhmMzA0MzYyOTFiN2JlZGE0QHJlZGVzLWFkbWluLmFwcHMub2NwLWRyLnRkaWdpdGFsLXZpdm8uY29tLmJy
kind: Secret
metadata:
  creationTimestamp: null
  name: apicast-configuration-url-secret
  namespace: redes-3scale-apicast-dev
type: Opaque
```
4. Save the output in this git repository in `deployments/secrets/envs/dr/000-apicast-configuration-url-secret.yaml`.

## DR pipeline configuration

### Create and run the ArgoCD pipeline for secrets

1. Use the ArgoCD operator to create a new ArgoCD Application using the manifest in `dr-secrets-pipeline.yaml`.
2. Sync the new application to create the APIcast secret.


### Create and run the ArgoCD pipeline for movel APIcasts

1. Use the ArgoCD operator to create a new ArgoCD Application using the manifest in `dr-apicast-movel-pipeline.yaml`.
2. Sync the new `dr-apicast-movel` application to create the APIcast instance.

## CTP secret configuration

### Create an access token in 3scale

1. Using the CTP 3scale Admin interface, navigate to Account Settings ==> Tokens
2. Add a new token with access to all APIs and Read/Write permissions.
3. Make a note of the generated token value.

### Create the secret for APIcast configuration

1. Assume that the access token value that we just created is `7ae6d59bdf549b3f407216ea8eb42a90c28a3c1d851e11c8f30436291b7beda4`
2. From a command line, execsute the following:

```
export TOKEN=7ae6d59bdf549b3f407216ea8eb42a90c28a3c1d851e11c8f30436291b7beda4
export THREESCALE_URL=redes-admin.apps.ocp-dr.tdigital-vivo.com.br
oc create secret generic apicast-configuration-url-secret --dry-run=client --from-literal=AdminPortalURL=https://${TOKEN}@${THREESCALE_URL} --type=Opaque -n redes-3scale-apicast -o yaml
```
3. The output should be similar to the following:
```
apiVersion: v1
data:
  AdminPortalURL: aHR0cHM6Ly83YWU2ZDU5YmRmNTQ5YjNmNDA3MjE2ZWE4ZWI0MmE5MGMyOGEzYzFkODUxZTExYzhmMzA0MzYyOTFiN2JlZGE0QHJlZGVzLWFkbWluLmFwcHMub2NwLWRyLnRkaWdpdGFsLXZpdm8uY29tLmJy
kind: Secret
metadata:
  creationTimestamp: null
  name: apicast-configuration-url-secret
  namespace: redes-3scale-apicast
type: Opaque
```
4. Save the output in this git repository in `deployments/secrets/envs/ctp/000-apicast-configuration-url-secret.yaml`.

## CTP pipeline configuration

### Create and run the ArgoCD pipeline for secrets

1. Use the ArgoCD operator to create a new ArgoCD Application using the manifest in `ctp-secrets-pipeline.yaml`.
2. Sync the new application to create the APIcast secret.


### Create and run the ArgoCD pipeline for movel APIcasts

1. Use the ArgoCD operator to create a new ArgoCD Application using the manifest in `ctp-apicast-movel-pipeline.yaml`.
2. Sync the new `ctp-apicast-movel` application to create the APIcast instance.
