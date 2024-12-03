---
{"dg-publish":true,"permalink":"/garden-notes/kube-controller-manager/","tags":["note","seedling"],"created":"2023-04-13","updated":"2024-11-29T14:50"}
---

# Kube-controller-manager

Controller is the process that continously monitors various components within the system and works towards bringing the whole system to the desired state. As an example:
- node-controller checks status of the nodes every 5 seconds (Node Monitor Period). If it stops reciving heartbear from the node, it waits 40 seconds (Node Monitor Grace Period) until marj the node as unreachable. Then it waits 5 minutes before it starts evicting pods (Pod Eviction TImeout). 
- replication-controller that monitors replicaset to run desired number of pods.
- Other controller examples are: deployment-controller, namespace-controller, job-controller, etc.

All those controllers are combinet as a singel process named kube-controller-manager. The kube-controller-manager can be run as:
- run as systemd service with a config file `/etc/systemd/system/kube-controller-manager.service`) 
- run as a kubeadmin controled pod with a config file `/etc/kubernetes/manifest/kube-controller-manager.yaml` 

---

