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