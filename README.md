# ToDo App Deployment with Helm & Kind

This project demonstrates an advanced Helm chart deployment for a multi-component application (Django Todolist + MySQL) onto a local `kind` (Kubernetes in Docker) cluster.

## 🎯 Project Goal

The primary goal was to create a deeply parameterized and production-ready Helm chart structure. The focus is not on the application itself, but on the orchestration and DevOps practices involved in its deployment.

## ⚙️ Key Features

This Helm setup demonstrates several key Kubernetes and Helm concepts based on the task requirements:

* **Cluster Setup:** Deploys a multi-node `kind` cluster from a `cluster.yml` configuration.
* **Scheduling (Taints & Tolerations):** The `kind` cluster nodes are tainted (`app=mysql:NoSchedule`), and the `mysql` sub-chart is configured with the corresponding tolerations to ensure pods land on the correct nodes.
* **Helm Sub-charts:** The main `todoapp` chart includes `mysql` as a chart dependency (`Chart.yaml`), managing a stateful application alongside a stateless one.
* **Full Parameterization:** All critical values are exposed in `values.yaml`, including:
    * Namespace, image tags, and replica counts.
    * Resource `requests` and `limits`.
    * `RollingUpdate` strategy parameters.
    * HPA `min`/`max` replicas and CPU/Memory targets.
    * PV/PVC storage sizes.
    * RBAC `ServiceAccount` names.
* **Dynamic Secrets:** Uses the Helm `range` function to dynamically populate Kubernetes secrets from `values.yaml`, which are then mounted into the deployment.
* **Automated Deployment:** A `bootstrap.sh` script is provided to automate the entire process: cluster creation, node tainting, and Helm installation.

## 🚀 How to Run

### Prerequisites

* `Docker`
* `kind`
* `kubectl`
* `Helm 3`

### 1. Clone the Repository

```
git clone git clone https://github.com/Muskam09/writing-helm-charts.git
cd writing-helm-charts
```

### 2. Make the Script Executable
``` chmod +x bootstrap.sh ```
**(Only needed once)**

### 3. Run the Deployment
The bootstrap.sh script handles everything. It will:
1. Create the kind cluster using cluster.yml.
2. Taint the appropriate nodes for MySQL.
3. Deploy any prerequisites (like a metrics server, if you added one).
4. Install the todoapp Helm chart with all its sub-charts.
```./bootstrap.sh```

### ⏹️ How to Stop (Cleanup)
The easiest way to remove everything is to delete the kind cluster:
```
kind delete cluster
```
