---
{"dg-publish":true,"permalink":"/garden-notes/taints-and-tolerations/","tags":["note","seedling"],"created":"2023-02-03T17:06:00","updated":"2024-11-29T14:52"}
---

# Taints and Tolerations

`Taints` allow a node to reject a set of pods. Taint can take three effects:

- `NoSchedule` - Kubernetes will not schedule the pod onto that node.
- `PreferNoSchedule` - Kubernetes will try to not schedule the pod onto the node.
- `NoExecute` - Kubernetes will not schedule the pod onto that node and if a pod is already running on a node it will be evicted (terminated).

`Tolerations` are applied to pods. Tolerations allow the scheduler to schedule pods with matching taints. Tolerations allow scheduling but **don't guarantee** scheduling (see _Figure 1_)

```mermaid
flowchart TD
	p("Pod") --> n1("Node1");
	p -..-x n2("Node2");
	p --> n3("Node3");
	
	classDef plain fill:#ddd,stroke:#fff,stroke-width:4px,color:#000;
	classDef plain-green fill:#ddd,stroke:#77DD77,stroke-width:5px,color:#000;
	classDef k8s fill:#326ce5,stroke:#fff,stroke-width:0px,color:#fff;
	classDef k8s-green fill:#326ce5,stroke:#77DD77,stroke-width:5px,color:#fff;
	classDef k8s-red fill:#326ce5,stroke:#F67280,stroke-width:5px,color:#fff;
	class n1 k8s-green;
	class n2 k8s-red;
	class n3 k8s;
	class p plain-green;
```
_Figure 1: Scheduling of a pod with taints an tolerations. Node1 has a taint `color=green:Noschedule`, Node2 has `color=red:Noschedule` and Node3 has no taint. Pod is configured with toleration accepting `color=green:NoSchedule`. Scheduler can place the pod on Node1 or Node3.

## Tainting a Node

Add taint:

```bash
kubectl taint nodes node1 key1=value1:NoSchedule
```

Remove taint:

```bash
kubectl taint nodes node1 key1=value1:NoSchedule-
```


## Adding Tolerations to a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  tolerations:
  - key: "key1"
	operator: "Equal"
	value: "value1"
	effect: "NoSchedule"
  - key: "key2"
    operator: "Exists"
    effect: "NoSchedule"
```





---
- https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/