---
{"dg-publish":true,"permalink":"/garden-notes/network-policies/","tags":["note","seedling"],"created":"2023-02-11","updated":"2024-11-29T14:51"}
---

# Network Policies

Network Policies allows to control network flow to (`policyTypes: Ingress`) and from (`policyTypes: Egress`) pods and they are attached to pods through `podSelector`. 

There are four identifiers that specify what pod can communicate:

- ports
- other pods
- namespaces
- IP blocks with specific ports

Network Policies are additive, means the connection allowed is the union of waht applicable pollicies allow, e.g. if there are two rules applied on a pod where one is denying connection and other is allowing it, the result will be allowed.

## Example Policy

The following policy is attached to all pods labeled `role=db` and it:

- allows incoming traffic on port TCP/80
- allows incoming traffic on ports TCP/6379-6380  from 
	- subnet 172.17.0.0/16 (excluding 172.17.1.0/24)
	- pods in namespace labeled  `project=myproject` 
	- pods labeled `role=frontend`
- allows outgoing traffic on port TCP/5978 to:
	- subnet 10.0.0.0/24
	- pods labaled `role: abc` **AND** running in a namespace labaled `project: xyz` (Note: both destinations are defined as a single array's element)


```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - ports:
	    - protocol: TCP
		  port: 80
    - from:
        - ipBlock:
            cidr: 172.17.0.0/16
            except:
              - 172.17.1.0/24
        - namespaceSelector:
            matchLabels:
              project: myproject
        - podSelector:
            matchLabels:
              role: frontend
	  ports:
        - protocol: TCP
          port: 6379
          endPort: 6380
          
  egress:
    - to:
        - namespaceSelector:
            matchLabels:
              project: xyz
          podSelector:
            matchLabels:
              role: abc
        - ipBlock:
            cidr: 10.0.0.0/24
      ports:
        - protocol: TCP
          port: 5978
```


## Default Policies

By default, if no policies exist in a namespace, then all ingress and egress traffic is allowed to and from pods in that namespace. The following examples let you change the default behavior in that namespace.

### Deny All Ingress Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-ingress
spec:
  podSelector: {}
  policyTypes:
  - Ingress
```

### Allow All Ingress Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-ingress
spec:
  podSelector: {}
  ingress:
  - {}
  policyTypes:
  - Ingress
```

### Deny All Egress Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-egress
spec:
  podSelector: {}
  policyTypes:
  - Egress
```


### Allow All Egress Traffic

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-all-egress
spec:
  podSelector: {}
  egress:
  - {}
  policyTypes:
  - Egress
```

---
- https://kubernetes.io/docs/concepts/services-networking/network-policies/
