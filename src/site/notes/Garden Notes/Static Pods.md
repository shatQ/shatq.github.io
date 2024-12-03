---
{"dg-publish":true,"permalink":"/garden-notes/static-pods/","tags":["note","seedling"],"created":"2023-06-05","updated":"2024-11-29T14:52"}
---

# Static Pods

- Static pods are created by kubelet
- The pod yaml definition file needs to be placed under static pods path e.g. `/etc/kubernetes/manifests`. The designated folder path is defined in:
	-  `kublet.service` as a direct option `--pod-manifest-path=/etc/kubernetes/manifests
	- `kublet.service` as indirect option `--config=kubeconfig.yaml` where `kubeconfig.yaml` defined `staticPodPath:/etc/kubernetes/manifests`
- Static pod name is automatically appended with a node name where the pod is running, e.g. `kube-apiserver-controlplane`
- The most common use case is kubeadm tool that setups cluster creating static pods for various controlplane components.

---

