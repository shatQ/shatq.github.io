---
{"dg-publish":true,"permalink":"/garden-notes/kubernetes-api/","tags":["note","seedling"],"created":"2023-09-20","updated":"2024-11-29T14:51"}
---

# Kubernetes API

## Accessing API

- API is listening on kube-apiserver port 6433
- To authenticate you need to pass user certificate:
```
$ curl http://localhost:6443 -k --key admin.key --cert admin.crt --cacert ca.crt
```
- An alternative way of authenticate is to use kubectl proxy:
```
$ kubectl proxy
Starting serve on 127.0.0.1:8001
$curl http://localhost:8001 -k
```
## API groups

Core group `/api`:
- /api/v1/namespaces
- /api/v1/pods
- /api/v1/rc
- /api/v1/events
- /api/v1/endpoints
- /api/v1/nodes
- /api/v1/bindings
- /api/v1/PV
- /api/v1/PVC
- /api/v1/configmaps
- /api/v1/secrets
- /api/v1/services

Named group `/apis`:
- /apps, e.g. /apps/v1/deployments
- /extensions
- /networking.k8s.io, e.g. /networking.k8s.io/v1/networkpolicies
- /storage.k8s.io
- /authentication.k8s.io
- /certificates.k8s.io, e.g. /certificates.k8s.io/v1/certificatesigningrequests

---

