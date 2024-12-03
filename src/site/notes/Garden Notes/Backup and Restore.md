---
{"dg-publish":true,"permalink":"/garden-notes/backup-and-restore/","tags":["note","seedling"],"created":"2023-06-16","updated":"2024-11-29T14:43"}
---

# Backup and Restore

## Backing up an etcd cluster

Etcd supports built-in snapshot. A snapshot may either be taken from a live member with the `etcdctl` snapshot save command or by copying the `member/snap/db` file from an etcd data directory that is not currently used by an etcd process. Taking the snapshot will not affect the performance of the member.

Taking a snapshot:

```
ETCDCTL_API=3 etcdctl --endpoints [https://127.0.0.1:2379] --cert /etc/kubernetes/pki/etcd/peer.crt --key /etc/kubernetes/pki/etcd/peer.key --cacert /etc/kubernetes/pki/etcd/ca.crt snapshot save /opt/snapshot-pre-boot.db
```

Restoring a snapshot:

```
ETCDCTL_API=3 etcdctl snapshot restore --data-dir="/var/lib/etcd-restored" /opt/snapshot-pre-boot.db
```

Reconfiguring a cluster to use etcd data from restored location. For cluster deployed with `kubeadmin` edit etcd manifest file `/etc/kubernetes/manifests/etcd.yaml` and update volume mount path:

```
  - hostPath:
      path: /var/lib/etcd-restored
      type: DirectoryOrCreate
    name: etcd-data
``` 

After updating the file, etcd pod will be re-created and it will use restored data.

## Backup resource configs

Backing up resource configs is method useful when we don't have access to etcd cluster, e.g. cloud managed kubernetes clusters. In such situation good option is to backup all services by querying `kube-apiserver`:

```
kubectl get all --all-namespaces -o yaml > all-deployed-services.yaml
```

---
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster
- https://github.com/etcd-io/website/blob/main/content/en/docs/v3.5/op-guide/recovery.md

