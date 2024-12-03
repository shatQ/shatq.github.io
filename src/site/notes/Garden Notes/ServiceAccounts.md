---
{"dg-publish":true,"permalink":"/garden-notes/service-accounts/","tags":["note","seedling"],"created":"2023-09-20","updated":"2024-11-29T14:52"}
---

# ServiceAccounts

## Overview

There are two types of accounts in Kubernetes:

- User - used by users
- Service - used by apps

## Creating Service Account

```
$ kubectl create serviceaccount dashboard-sa
```

```
$ kubectl describe serviceaccount dashboard-sa
Name:                    dashboard-sa
Namespace:               default
Labels:                  <none>
Annotations:             <none>
Image pull secrets:      <none>
Mountable secrets:       dashboard-sa-token-kbbdm
Tokens:                  dashboard-sa-token-kbbdm
Events:                  <none>
```

## Secret Token

When service account is being created, kubernetes creates token and store it as a secret object. The secret object is then linked with a service account. The token needs to be exported and used as an authentication baerer token when external application calls kubernetes API.

If application that uses service account is located inside the cluster, there is no need of exporting the token. The secret containing the token can be simply mounted as a volume to the application pod.

**Note:** v1.22 TokenRequestAPI token was introduced that is used as projected volume and mounted to the pod. The advantage is that TokenRequestAPI generated token has expiration time defined.

**Note:** v1.24 Reduction of Secret-based SA Tokens. When service account is created, kubernetes no longer create secret token. To create it you need to run token command  explicitly `kubectl create token dashboard-sa`. 

---

