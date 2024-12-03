---
{"dg-publish":true,"permalink":"/garden-notes/private-docker-repository-credentials/","tags":["note","seedling"],"created":"2023-09-22T16:35:00","updated":"2024-11-29T14:51"}
---

# Private Docker Repository Credentials

If image is not specified with full path, kubernetes assumes docker hub as a default registry, e.g. `nginx` -> `docker.io/library/nginx` .

To use image that origins from private registry, you need to specify the full path, e.g. `private-registry.io/apps/internal-app`.

To authenticate private registry, the credentials needs to be stored as secret object:

```
kubectl create secret docker-registry regcred \
	--docker-server=private-registry.io \
	--docker-username=registry-user \
	--docker-password=registry-password \
	--docker-email=registry-user@org.com
```

Then, in pod definition you need to reference image secrets in the following way:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: private-registry.io/apps/internal-app
  imagePullSecrets:
  - name: regcred
```


---
