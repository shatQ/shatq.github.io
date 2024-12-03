---
{"dg-publish":true,"permalink":"/garden-notes/dns/","tags":["note","seedling"],"created":"2023-10-17 16:35","updated":"2024-11-29T14:50"}
---

# DNS

- DNS within the kubernetes cluster allows for services and pods name resolution across nodes. The DNS is served by `CoreDNS` service.
- Service names are resolved automatically. To resolve pods, the proper option needs to be enabled in kubernetes DNS settings. Pods resolution name is  pod's ip address where dots are replaced with hyphens.
- Example FQDN:
  
| Hostname    | Namespace | Type | Root          | IP Address    |
| ----------- | --------- | ---- | ------------- | ------------- |
| web-service | apps      | svc  | cluster.local | 10.107.37.188 |
| 10-244-2-5  | default   | pod  | cluster.local | 10.244.2.5    |

- Example search:

```
$ curl http://web-service.apps
$ curl http://web-service.apps.svc
$ curl http://web-service.apps.svc.cluster.local
```

---
https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/
https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/
