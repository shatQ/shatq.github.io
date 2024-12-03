---
{"dg-publish":true,"permalink":"/garden-notes/kube-apiserver/","tags":["note","seedling"],"created":"2023-04-05","updated":"2024-11-29T14:50"}
---

# Kube-apiserver

When you running `kubectl` command, e.g. to create a new pod, the following is happening:
- request is passed to `kube-apiserver` that:
	- authenticates
	- validates request
	- retrieves data
	- updates etcd
- `kube-scheduler` identifies right node to place new pod and communicates it to `kube-apiserver` 
- `kube-apiserver` and passes information to `kubelet` on apriopriate worker node
- `kubelet` creates a pod and infroms kube-apiserver about the status

Kube-apiserve can be:
- run as systemd service with a config file `/etc/systemd/system/kube-apiserver.service`) 
- run as a kubeadmin controled pod with a config file `/etc/kubernetes/manifest/kube-apiserver.yaml` 

---
references:

