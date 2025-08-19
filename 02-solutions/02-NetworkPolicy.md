# Exercise: Configure Network in OpenShift

**Objective:** Set up an network policies between projects.

## Requirements and Steps

Project frontend with:
- Pod nginx labeled **run=nginx**
- Pod busybox labeled **run=not-nginx**

Project backend with:
- Pod httpd labeled **run=httpd**

### 1. Deny all ingress traffic to the backend namespace by default

Firstly, we should ensure that communication is possible between pods.

```bash 
oc exec -n frontend pod/nginx -- curl -s httpd.backend.svc.cluster.local

We should get 
**It works!**

oc exec -n frontend pod/busybox -- curl -s httpd.backend.svc.cluster.local

We should get
**It works!**
```

Ensure that there are no network policies in place. View and delete any old netpol.

```bash
oc get netpol -n backend

NAME                           POD-SELECTOR   AGE
allow-from-all-namespaces      <none>         19m
allow-from-ingress-namespace   <none>         19m

oc delete netpol allow-from-all-namespaces -n backend

networkpolicy.networking.k8s.io "allow-from-all-namespaces" deleted

oc delete netpol allow-from-ingress-namespace -n backend

networkpolicy.networking.k8s.io "allow-from-ingress-namespace" deleted
```

**Without a network policy, everything is allowed by default**

```bash

# vim ex280-DenyAllBackend.yaml 
```

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: ex280-DenyAllBackend
  namespace: backend
spec:
  podSelector: {}
  policyTypes:
    - Ingress
status: {}
```
```bash
oc apply -f ex280-DenyAllBackend.yaml
```

We can't connect from another pod with network policy applied.

```bash 
oc exec -n frontend pod/nginx -- curl -s httpd.backend.svc.cluster.local

We should get 
**error**
#revisar
oc exec -n frontend pod/busybox -- curl -s httpd.backend.svc.cluster.local

We should get
**error**
#revisar
```

### 2. Create a NetworkPolicy in backend that:
  - Applies only to pods with the label run=httpd
  - Allows only TCP traffic on port 80
  - Allows ingress only from pods in frontend with the label run=nginx

```bash

# vim ex280-AllowFrontFrontendLabeled.yaml
```

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: AllowFrontFrontendLabeled
  namespace: backend
spec:
  podSelector:
    matchLabels:
      run: httpd
  ingress:
    - ports:
        - protocol: TCP
          port: 80
      from:
        - podSelector:
            matchLabels:
              run: nginx
          namespaceSelector:
            matchLabels:
              kubernetes.io/metadata.name: frontend
  policyTypes:
    - Ingress
status: {}
```

Finally, we can perform the following validation:

```bash 
oc exec -n frontend pod/nginx -- curl -s httpd.backend.svc.cluster.local

We should get 
**It works!**
#revisar
oc exec -n frontend pod/busybox -- curl -s httpd.backend.svc.cluster.local

We should get
**Error**
#Revisar
```