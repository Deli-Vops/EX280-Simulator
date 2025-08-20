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

### 2. Group Management
- Create a group named `frontends` and `backend`.
- Add users `alice` to the `frontends` group.
- Add users `charlie` to the `backend` group.

### 3. Project and RBAC
- Assign the following roles in the `apollo` project:
  - Group `frontends`: **edit**
  - User `charlie`: **view**
- Assign the following roles in the `hades` project:
  - Group `frontends`: **admin**
- Assign the **cluster-admin** role in the `bob`user.

## Notes
- Users can log in via `oc login` using their HTPasswd credentials.
- The Identity Provider is configured through an `OAuth` resource referencing the secret.
