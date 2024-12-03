---
{"dg-publish":true,"permalink":"/garden-notes/rbac/","tags":["note","seedling"],"created":"2023-02-02T17:34:00","updated":"2024-11-29T14:52"}
---

# RBAC

## Overview

To enable RBAC, start the Kubernetes API server with the `--authorization-mode` flag set.

An RBAC Role or _ClusterRole_ contains rules that represent a set of permissions. Permissions are purely additive (there are no "deny" rules).

A _Role_ always sets permissions within a particular namespace; when you create a _Role_, you have to specify the namespace it belongs in.

_ClusterRole_, by contrast, is a none-namespaced resource. The resources have different names (_Role_ and _ClusterRole_) because a Kubernetes object always has to be either namespaced or not namespaced. It can't be both.

_ClusterRoles_ have several uses. You can use a _ClusterRole_ to:

- define permissions on namespaced resources and be granted access within individual namespace(s)
- define permissions on namespaced resources and be granted access across all namespaces
- define permissions on cluster-scoped resources

If you want to define a role within a namespace, use a _Role_; if you want to define a role cluster-wide, use a _ClusterRole_.

A _RoleBinding_ may reference any role in the same namespace. Alternatively, a _RoleBinding_ can reference a _ClusterRole_ and bind that _ClusterRole_ to the namespace defined in the _RoleBinding_. This kind of reference lets you define a set of common roles across your cluster, then reuse them within multiple namespaces.

## Creating a Role

**Note:** For core API group specify `[""]`.

```
$ vim developer-role.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```

```
$ kubectl create -f developer-role.yaml
```

## Binding a Role

```
$ vim devuser-developer-binding.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```

```
$ kubectl create -f devuser-developer-binding.yaml
```

## View RBAC

```
$ kubectl get roles
```

```
$ kubectl get rolebindings
```

```
$ kubectl describe role developer
```


## Check Access

```
$ kubectl auth can-i create deployments
```


## Creating a ClusterRole

To see cluster scoped resources:

```
$ kubectl api-resources --namespaced=false
```

```
$ vim cluster-admin-role.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "delete"]
```

```
$ kubectl create -f cluster-admin-role.yaml
```


## Binding a ClusterRole

```
$ vim cluster-admin-role-binding.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administrator
  apiGroup: rbac.authorization.k8s.io
```

```
$kubectl create -f cluster-admin-role-binding.yaml
```

---
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/