---
{"dg-publish":true,"permalink":"/garden-notes/container-runtimes/","tags":["note","seedling"],"created":"2023-04-05","updated":"2024-11-29T14:43"}
---

# Container Runtimes

- Kubernetes was born initialy to orchestrates docker containers only. 
- Over time, as more and more container runtimes were introduced, Kubernetes implemented the [Container Runtime Interface (CRI)](https://kubernetes.io/docs/concepts/architecture/cri/) to allow use a wide variety of container runtimes.
- CRI supports all runtimes that are compatibile with [Open Container Initiative (OCI)](https://www.opencontainers.org/)
- Because docker was created before OCI was introduced, it was not compatibile with OCI and thus direct kubernetes integration was supported till version v1.24.
- Since v1.24 docker itegrates with kubernetes through [containerD](https://containerd.io/).


---
