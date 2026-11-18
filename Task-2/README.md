# 🚀 Task 2: FastAPI + MySQL with ConfigMap, Secret & Ingress

## 📝 Overview

In this task, a **FastAPI** guestbook application (`webapp-api`) backed by a **MySQL 8.4** database was deployed onto a 3-node Minikube Kubernetes cluster inside the `webapp-stack` namespace[cite: 1].

### Architecture & Implemented Resources
- **Namespace:** `webapp-stack` isolating all namespaced application resources[cite: 1].
- **Configuration & Secrets:** `ConfigMap` (`webapp-config`) for application settings and `Secret` (`mysql-secret`) for database credentials[cite: 1].
- **Persistent Storage:** `PersistentVolume` (`mysql-pv`, `2Gi`, `storageClassName: manual`, pinned to worker node `minikube-m02` via `nodeAffinity`) bound to `PersistentVolumeClaim` (`mysql-pvc`)[cite: 1].
- **Database Tier:** `Deployment` (`mysql`, 1 replica, `Recreate` strategy, `readinessProbe` & `livenessProbe`) exposed internally via `ClusterIP` `Service` (`mysql` on port `3306`)[cite: 1].
- **Application Tier:** `Deployment` (`webapp-api`, 2 replicas, HTTP `/health` probes) exposed via `ClusterIP` `Service` (`webapp-api` mapping port `80` → `8000`)[cite: 1].
- **External Routing:** NGINX Ingress Controller with `Ingress` (`webapp-ingress`) routing `/` traffic to `webapp-api:80`[cite: 1].

---

## 📁 Repository Structure

```text
Task-2/
├── 01-namespace.yaml      # webapp-stack namespace
├── mysql-secret.yaml      # Database credentials (Secret)
├── webapp-config.yaml     # App & DB environment settings (ConfigMap)
├── mysql-pv.yaml          # HostPath PersistentVolume pinned to minikube-m02
├── mysql-pvc.yaml         # PersistentVolumeClaim (2Gi)
├── mysql.yaml             # MySQL Deployment & ClusterIP Service
├── webapp.yaml            # FastAPI Deployment (2 replicas) & ClusterIP Service
├── ingress.yaml           # NGINX Ingress resource
└── README.md              # Documentation & verification screenshots
```

---

## 🔍 Verification & Screenshots

### 1. Docker Images Built & Loaded into Minikube
Built `webapp-api:task2` and `webapp-mysql:task2` locally and loaded both into Minikube (`minikube image ls`)[cite: 1]:

<img width="819" height="350" alt="Screenshot From 2026-11-17 15-12-45" src="https://github.com/user-attachments/assets/39959b8c-922a-4319-a8ad-f5131e71157b" />

---

### 2. Secret & ConfigMap Created
Verified `mysql-secret` (`DATA: 3`) and `webapp-config` (`DATA: 5`) inside `webapp-stack`[cite: 1]:

<img width="949" height="205" alt="Screenshot From 2026-11-17 15-14-00" src="https://github.com/user-attachments/assets/8adc7819-7197-492a-8f45-20413f6fa918" />

---

### 3. PersistentVolume & PersistentVolumeClaim Bound
Verified `mysql-pv` and `mysql-pvc` are `Bound` with `2Gi` capacity under `manual` storage class[cite: 1]:

<img width="1500" height="154" alt="Screenshot From 2026-11-17 15-14-38" src="https://github.com/user-attachments/assets/59f77e4b-e357-4834-b1ae-22e1338fddf0" />

---

### 4. Deployments Ready
Verified both `mysql` (`1/1`) and `webapp-api` (`2/2`) deployments are up and available[cite: 1]:

<img width="960" height="114" alt="Screenshot From 2026-11-17 15-15-11" src="https://github.com/user-attachments/assets/675124e9-1c7a-498e-b0ed-9454e5b07987" />

---

### 5. MySQL Scheduled on PV Node (`minikube-m02`)
Verified via `nodeAffinity` that the `mysql` pod is running specifically on worker node `minikube-m02`[cite: 1]:

<img width="1263" height="100" alt="Screenshot From 2026-11-17 15-15-55" src="https://github.com/user-attachments/assets/ef9b2870-9219-49c5-8f56-e6e6749cc2cc" />

---

### 6. Application Environment Variables Injected
Checked environment variables inside `deploy/webapp-api` confirming values from both `webapp-config` and `mysql-secret`[cite: 1]:

<img width="1160" height="874" alt="Screenshot From 2026-11-17 15-16-42" src="https://github.com/user-attachments/assets/e439232f-5786-421d-92a8-f1301d6f2872" />

---

### 7. Initial Database Schema & Default Data
Queried `webapp.messages` inside the MySQL container confirming initialization created the 2 default messages[cite: 1]:

<img width="1897" height="178" alt="Screenshot From 2026-11-17 15-30-11" src="https://github.com/user-attachments/assets/9f324d8f-2e7f-4555-ad44-0c0b863fafd1" />

---

### 8. Ingress Configured
Verified `webapp-ingress` is using `nginx` class with an assigned address[cite: 1]:

<img width="1112" height="83" alt="Screenshot From 2026-11-17 15-30-45" src="https://github.com/user-attachments/assets/a1939045-e66f-4841-b949-95bb1a8515e0" />

---

### 9. Application Info Endpoint (`/api/info`)

**Note:** Local port `8081` was used for `kubectl port-forward` (`8081:80`) because port `8080` is reserved for Jenkins on the host machine.

<img width="1898" height="73" alt="Screenshot From 2026-11-17 15-31-43" src="https://github.com/user-attachments/assets/3b2bfc48-e222-4388-8580-72da0aeff2a1" />

---

### 10. Writing a New Message (`POST /api/messages`)
Posted a new guestbook entry via the Ingress endpoint[cite: 1]:

<img width="1847" height="59" alt="Screenshot From 2026-11-17 15-32-53" src="https://github.com/user-attachments/assets/d060ec00-ea22-4f8b-a978-2582b243e73c" />

---

### 11. Reading Messages (`GET /api/messages`) & Persistence Check
Verified all messages (the 2 default entries plus the newly posted entry) are returned from MySQL[cite: 1]:

<img width="1919" height="132" alt="Screenshot From 2026-11-17 15-33-38" src="https://github.com/user-attachments/assets/31cfdbbc-acb0-4dda-9be4-350aa93b9004" />
