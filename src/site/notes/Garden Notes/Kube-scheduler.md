---
{"dg-publish":true,"permalink":"/garden-notes/kube-scheduler/","tags":["note","seedling"],"created":"2023-04-13","updated":"2024-11-29T14:50"}
---

# Kube-scheduler

The kube-scheduler decides on witch node the pod should be running. 

Kube-scheduler can be:
- run as systemd service with a config file `/etc/systemd/system/kube-scheduler.service`
- run as a kubeadmin controled pod with a config file `/etc/kubernetes/manifest/kube-scheduler.yaml` 

---
