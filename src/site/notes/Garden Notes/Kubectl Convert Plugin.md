---
{"dg-publish":true,"permalink":"/garden-notes/kubectl-convert-plugin/","tags":["note","seedling"],"created":"2023-01-30T14:31:00","updated":"2024-11-29T14:51"}
---

# Kubectl Convert Plugin

A plugin for Kubernetes command-line tool `kubectl`, which allows you to convert manifests between different API versions. This can be particularly helpful to migrate manifests to a non-deprecated api version with newer Kubernetes release. For more info, visit [migrate to non deprecated apis](https://kubernetes.io/docs/reference/using-api/deprecation-guide/#migrate-to-non-deprecated-apis)

## Download the latest release
 
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert"
```

## Validate the binary (optional)

Download the kubectl-convert checksum file: 


```bash
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert.sha256"
```


Validate the kubectl-convert binary against the checksum file:

```bash
echo "$(cat kubectl-convert.sha256) kubectl-convert" | sha256sum --check
```
 
If valid, the output is:

```console
kubectl-convert: OK
```

If the check fails, `sha256` exits with nonzero status and prints output similar to:

```bash
kubectl-convert: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
```

>[!NOTE] 
>Download the same version of the binary and checksum.
 
## Install kubectl-convert

```bash
sudo install -o root -g root -m 0755 kubectl-convert /usr/local/bin/kubectl-convert
```

## Verify plugin is successfully installed

```shell
kubectl convert --help
```

If you do not see an error, it means the plugin is successfully installed.

---
- https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-convert-plugin


