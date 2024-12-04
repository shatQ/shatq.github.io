---
{"dg-publish":true,"permalink":"/mo-cs/kubernetes-moc/","tags":["moc"],"created":"2024-11-28T10:21","updated":"2024-11-28T21:17"}
---

# Kubernetes MOC

## Architecture

- [[Garden Notes/Etcd\|Etcd]] - It is a strongly consistent, distributed key-value store that stores information about the cluster.
- [[Garden Notes/Kube-scheduler\|Kube-scheduler]] - Responsible for scheduling application containers on worker nodes
- [[Garden Notes/Kube-controller-manager\|Kube-controller-manager]] - Take care of different cluster functions, e.g. node controller, replication controller, etc.
- [[Garden Notes/Kube-apiserver\|Kube-apiserver]] - Responsible for orchestrating all operations within the cluster.
- [[Garden Notes/Kubelet\|Kubelet]] - Listens for instructions from kube-apiserver and manages containers.
- [[Garden Notes/Kube-proxy\|Kube-proxy]] - Helps enabling communication between services within the cluster.
- [[Garden Notes/Container Runtimes\|Container Runtimes]]
- [[Garden Notes/Kubernetes API\|Kubernetes API]]
- [[Garden Notes/Static Pods\|Static Pods]]
- [[Garden Notes/Init Containers\|Init Containers]]
## Security

- [[Garden Notes/RBAC\|RBAC]]
- [[Garden Notes/ServiceAccounts\|ServiceAccounts]]
## Network

- [[Garden Notes/DNS\|DNS]]
- [[Garden Notes/Ingress\|Ingress]]
- [[Garden Notes/Network Policies\|Network Policies]]
- [[Garden Notes/Services\|Services]]
## Scheduling

- [[Garden Notes/Node Affinity\|Node Affinity]]
- [[Garden Notes/Node Selector\|Node Selector]]
- [[Garden Notes/Node Name\|Node Name]]
- [[Garden Notes/Taints and Tolerations\|Taints and Tolerations]]
## Maintenance

- [[Garden Notes/Cluster Upgrades\|Cluster Upgrades]]
- [[Garden Notes/Backup and Restore\|Backup and Restore]]
## Tools

- [[Garden Notes/kubectl-cheatsheet\|kubectl-cheatsheet]]
- [[Garden Notes/Kubectx and Kubens\|Kubectx and Kubens]]
- [[Garden Notes/Kubectl Convert Plugin\|Kubectl Convert Plugin]]
## Other

-  [[Private Notes/CKAD Simulator Kubernetes 1.26\|CKAD Simulator Kubernetes 1.26]]