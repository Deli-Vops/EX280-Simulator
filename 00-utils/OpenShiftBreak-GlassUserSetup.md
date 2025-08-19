# OpenShift Break-Glass User Setup (HTPasswd IdP)

This guide explains how to create a **break-glass user** (emergency account) in OpenShift using an HTPasswd Identity Provider and grant it `cluster-admin` privileges.

---

## 1. Create an htpasswd file

```bash
htpasswd -c -B htpasswd-brake-glass adminuser YourSafetyPasswordHere
```

> -c: create the file (overwrites if it already exists).

> -B: use bcrypt for secure hashing.

**Result:** 
File htpasswd-brake-glass containing user **adminuser** with password **YourSafetyPasswordHere** (hashed).

---

## 2. Create a secret in openshift-config

```bash
oc create secret generic htpasswd-brake-glass --from-file=htpasswd=htpasswd-brake-glass -n openshift-config
```

Creates a secret called htpasswd-brake-glass.

The key htpasswd contains the file content.

The secret will be referenced by the authentication operator.

---

## 3. Configure the OAuth Identity Provider

Edit the OAuth cluster configuration:

```bash
oc edit oauth cluster
```
Append this to .spec.identityProviders:

```yaml
- name: brake-glass
  mappingMethod: claim
  type: HTPasswd
  htpasswd:
    fileData:
      name: htpasswd-brake-glass
```

fileData.name: references the secret created in step 2. If you dont change any command, it will works (I hope so).

name: brake-glass: label shown on the login page. We want a descriptive name.

mappingMethod: claim: keeps the same username from the IdP.

---

After saving, the authentication operator redeploys pods in openshift-authentication.

The new **brake-glass** login option will appear in the web console.

---

## 4. Grant cluster-admin privileges

Creates/updates a ClusterRoleBinding granting cluster-admin to adminuser.

```bash
oc adm policy add-cluster-role-to-user cluster-admin adminuser
```

This applies immediately, even if the user hasn’t logged in yet.

---

If you haven't logged in with your new username yet, a warning will be displayed. Don't worry about this.

---

## 5. Time to test if this work

```bash
oc login -u adminuser -p YourSafetyPasswordHere

oc whoami

# should return: adminuser

oc auth can-i '*' '*' --all-namespaces
# should return: yes
```

---

## Outcome

- Full administrative privileges with cluster-admin.

## Best Practices

If you are using this procedure in a real environment, please: 
- Use a stronger password than **YourSafetyPasswordHere**.
- Store the htpasswd file and Secret securely.
- Limit use of the break-glass account to emergencies.
- Document and audit its usage.
- Rotate the password periodically.
