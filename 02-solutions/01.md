# Exercise: Configure HTPasswd Identity Provider, Users, Groups, and RBAC in OpenShift

**Objective:** Set up an HTPasswd Identity Provider, manage users and groups, and assign RBAC permissions to a project.

## Requirements and Steps

### 1. Identity Provider
- Create an HTPasswd Identity Provider named `ex280-htpasswd`.
- Store credentials in a **secret** named `ex280-htpasswd` in the `openshift-config` namespace.
- Create the following users with passwords equal to their usernames:
  - `alice`
  - `bob`
  - `charlie`

---

```bash
htpasswd -c -B -b ex280.htpasswd charlie charlie
htpasswd -B -b ex280.htpasswd alice alice
htpasswd -B -b ex280.htpasswd bob bob

oc create secret generic ex280-htpasswd --from-file=htpasswd=ex280.htpasswd -n openshift-config 

# vim ex280-htpasswd.yaml 
```
```yaml
apiVersion: config.openshift.io/v1
kind: OAuth
metadata:
  name: cluster
spec:
  identityProviders:
  - name: ex280-htpasswd 
    mappingMethod: claim
    type: HTPasswd
    htpasswd:
      fileData:
        name: htpasswd
```
```bash
# Check

oc apply -f ex280-htpasswd.yaml
```

### 2. Group Management
- Create a group named `frontends` and `backend`.
- Add users `alice` to the `frontends` group.
- Add users `charlie` to the `backend` group.

```bash
oc adm groups new frontends alice
oc adm groups new backend charlie
```

### 3. Project and RBAC
- Assign the following roles in the `apollo` project:
  - Group `frontends`: **edit**
  - User `charlie`: **view**
- Assign the following roles in the `hades` project:
  - Group `frontends`: **admin**
- Assign the **cluster-admin** role in the `bob`user.

```bash
oc adm policy add-role-to-group edit frontends -n apollo
oc adm policy add-role-to-user view charlie -n apollo
oc adm policy add-role-to-group admin frontends -n hades
oc adm policy add-cluster-role-to-user cluster-admin bob
```

### 4. Time to test login

```bash
oc login -u alice -p alice
oc login -u bob -p bob
oc login -u charlie -p charlie
```