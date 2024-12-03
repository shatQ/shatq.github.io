---
{"dg-publish":true,"permalink":"/garden-notes/kube-proxy/","tags":["note","seedling"],"created":"2023-04-13","updated":"2024-11-29T14:50"}
---

# Kube-proxy

Kube-proxy is process runing on each node that ensures that defined services are reachable accross the kubernetes cluster. It monitors services and for each newly created service it adds proper iptable rules.

![Pasted image 20230413142054.png](/img/user/_attachments/Pasted%20image%2020230413142054.png)

Kube-proxy can be:
- run as systemd service with a config file `/etc/systemd/system/kube-proxy.service`
- run as a kubeadmin controled deamon-set

---

