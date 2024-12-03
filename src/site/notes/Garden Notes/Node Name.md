---
{"dg-publish":true,"permalink":"/garden-notes/node-name/","tags":["note","seedling"],"created":"2023-02-09","updated":"2024-11-29T14:51"}
---

# Node Name

The `nodeName` is direct node selection that refers to node's name. Using nodeName overrules using nodeSelector or affinity and anti-affinity rules. 

Some of limitations:
-   If the named node does not exist, the Pod will not run, and in some cases may be automatically deleted.
-   If the named node does not have the resources to accommodate the Pod, the Pod will fail and its reason will indicate why, for example OutOfmemory or OutOfcpu.
-   Node names in cloud environments are not always predictable or stable.

## Adding nodeName to a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
  nodeName: kube-01
```

---
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename