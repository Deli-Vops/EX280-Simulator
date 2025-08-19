# Exercise: Configure Network in OpenShift

**Objective:** Set up an network policies between projects.

## Requirements and Steps

Project frontend with:
- Pod nginx labeled **run=nginx**
- Pod busybox labeled **run=not-nginx**
Project backend with:
- Pod httpd labeled **run=httpd**

### 1. Deny all ingress traffic to the backend namespace by default

### 2. Create a NetworkPolicy in backend that:
  - Applies only to pods with the label run=httpd
  - Allows only TCP traffic on port 80
  - Allows ingress only from pods in frontend with the label run=nginx

## Notes

Finally, we can perform the following validation:

```bash 
oc exec -n frontend pod/nginx -- curl -s httpd.backend.svc.cluster.local

We should get 
**It works!**


oc exec -n frontend pod/busybox -- curl -s httpd.backend.svc.cluster.local

We should get
**Error**
#Revisar
```