# Cluster Setup

This guide provides the YAML files you need to prepare a cluster for EX280.

---

## 1. Create projects that you are going to use

---

### Disclaimer

Be careful with this command. It will be **delete** any project with the same name.

Read it **carefully**. I dont want any tears. 

---

```bash
for projects in apollo hades frontend backend grapes ghosts confucio napoleon videoclub duck bull wheel comanche; do oc delete project $projects --ignore-not-found=true && oc new-project $projects;done
```

## 2. Deployments everywhere...

There are a lot of YAML files in the ClusterSetup folder.

Use the following command:

```bash
oc apply -f file
````

And apply all YAML.

Come, do it.

## 3. Network policies

We require multiple pods for the network policies exercise. Run the following commands to create them. We are using Nginx pods because they are the faster option.

```bash
#Frontend
oc run nginx --image=nginx --restart=Always --port=80 -l run=nginx -n frontend
oc expose pod nginx --port=80 --target-port=80 -n frontend
oc run busybox --image=busybox --restart=Never -l run=not-nginx -n frontend -- sleep 3600

#Backend
oc run httpd --image=httpd --restart=Always --port=80 -l run=httpd -n backend
oc expose pod httpd --port=80 --target-port=80 -n backend
```