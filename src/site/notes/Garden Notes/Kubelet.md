---
{"dg-publish":true,"permalink":"/garden-notes/kubelet/","tags":["note","seedling"],"created":"2023-04-13","updated":"2024-11-29T14:51"}
---

# Kubelet

Kubelet is running on each worker node and tho the following tasks:
- Registers a node within kubernetes cluster
- Create pods on kube-apiserver request
- Monitors node and pods status and reports it to kube-apiserver

Kublet must be always installed and run as systemd service with a config file `/etc/systemd/system/kube-scheduler.service`. 

---

