# Exercise: Create a Deployment with ConfigMap/Secret and Probes in OpenShift

**Objective:** Deploy an application in OpenShift using environment variables from a ConfigMap, Secret, and configure liveness and readiness probes.

## Requirements and Steps

- A project named grapes.

### 1. ConfigMap

Create a configmap named **grapes-config** containing:

```bash
Key: APP_MESSAGE
Value: "Hello from Grapes!"
```

### 2. Secret

Create a secret named **grapes-secret** containing:

```bash
Key: DB_PASSWORD
Value: "MySuperSecretPassword"
```

### 3. Deployment

Create a deployment named grapes-app in the grapes project. Label it with **app=grapes**.
- It will be use the **httpd:latest** image.
- Mount the **APP_MESSAGE** environment variable  from the configmap into the container.
- Mount the **DB_PASSWORD** environment variable from the secret into the container.

### 4. Probes

Configure readiness probe to check:

```bash
path: /
port: 8080
initialDelaySeconds: 10
timeoutSeconds: 3
periodSeconds: 8
failureThreshold: 5
```
Configure liveness probe to check:

```bash
path: /
port: 8080
initialDelaySeconds: 15
timeoutSeconds: 5
periodSeconds: 10
failureThreshold: 7
```

### 5. Service

Expose the deployment as a Service named **grapes-svc**.

## Notes

You can verify the Deployment and Service with:

```bash
oc get pods -n grapes
oc get svc -n grapes
```

Test the probes by checking pod status:

```bash
oc describe pod <pod_name> -n grapes
```