# EX280 Simulator

Welcome to the **EX280 Simulator**!  
This repository contains hands-on exercises to help you prepare for the **EX280 OpenShift exam**. Each exercise has a file with the practice (`01-exercises`) and its corresponding solution (`02-solutions`).

---

## Repository Structure

- `00-utils/` → Markdown files and YAML manifests to prepare your OpenShift cluster. Run these first to get your cluster ready.

| #  | Setup | Setup Link|
|----|----------|---------------|
| 01 | ClusterSetup | [ClusterSetup.md](00-utils/ClusterSetup.md) |
| 02 | OpenShiftBreak-GlassUserSetup | [OpenShiftBreak-GlassUserSetup.md](00-utils/OpenShiftBreak-GlassUserSetup.md) |

- `01-exercises/` → Exercise `.md` files.
- `02-solutions/` → Solution `.md` files corresponding to each exercise.

---

## Exercises List

| #  | Exercise | Exercise Link | Solution Link |
|----|----------|---------------|---------------|
| 01 | Htpasswd IDP | [01.md](01-exercises/01.md) | [01.md](02-solutions/01.md) |
| 02 | Network Policy | [02.md](01-exercises/02.md) | [02.md](02-solutions/02.md) |
| 03 | Deployment, Probe, ConfigMap & Secret | [03.md](01-exercises/03.md) | [03.md](02-solutions/03.md) |
| 04 | Manual Scale | [04.md](01-exercises/04.md) | [04.md](02-solutions/04.md) |
| 05 | HPA | [05.md](01-exercises/05.md) | [05.md](02-solutions/05.md) |
| 06 | Must-Gather | [06.md](01-exercises/06.md) | [06.md](02-solutions/06.md) |
| 07 | LimitRange & Resource Quotas | [07.md](01-exercises/07.md) | [07.md](02-solutions/07.md) |
| 08 | A Broken Deployment | [08.md](01-exercises/08.md) | [08.md](02-solutions/08.md) |
| 09 | SCC | [09.md](01-exercises/09.md) | [09.md](02-solutions/09.md) |
| 10 | Helm | Pending | Pending |
| 11 | Route | Pending | Pending |
| 12 | template projects | Pending | Pending |
| 13 | Deployment with NFS | Pending | Pending |

---

## How to Use This Simulator

##### 1. Clone the repository

##### 2. Prepare the cluster
Read all the .md files carefully and run the scripts and YAMLs in the 00-utils folder to prepare your OpenShift cluster.

##### 3. Enjoy

### Notes
Work on one exercise at a time and verify your resources using oc get and oc describe.

Explore events and logs in OpenShift to understand pod failures.

