# Kubernetes 3-Tier Application Architecture

### 📌 Project Overview
This project demonstrates a **basic 3-tier application architecture on Kubernetes,** designed purely for learning Kubernetes internals and real-world deployment patterns.

**Architecture Components:**
1. **Frontend:** Nginx (NodePort Service)
2. **Backend:** Node.js (ClusterIP Service)
3. **Database:** MySQL (StatefulSet + PVC + ConfigMap + Secret + Headless Service)

> Note:
> No business logic is implemented intentionally.
> The focus is on Kubernetes concepts, networking, and service-to-service connectivity — not application development.

### 🧩 Architecture
![diagram](/assets/architect-diagram.png)

---


### 🛠️ Prerequisites

Before you start, make sure you have installed:
| Tool        | Purpose                   | Documentation |
|-------------|-------------------------- |---------------|
| **kubectl** | Kubernetes CLI            | [Install kubectl](https://kubernetes.io/docs/tasks/tools/) |
| **KIND**    | Kubernetes local cluster  | [Install Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) |


### 🎯 Project Objectives
- Understand how **Kubernetes components interact**
- Implement a **real-world 3-tier architecture**
- Practice **service discovery using Kubernetes DNS**
- Learn differences between **stateless vs stateful workloads**
- Use **ConfigMaps & Secrets** in applications
- Validate **end-to-end communication**
- Build a **safe learning environment** (not production-ready)


### 🚀 Deployment Steps
**Step-1: Create Namespace**
```sh
kubectl apply -f namespace.yaml
```

**Step-2: Deploy Database (must be ready before backend)**
```sh
# Deploy database
kubectl apply -f database.yaml

# Test database connection
kubectl exec -it mysql-0 -n dev -- mysql -u appuser -p

# Check database
SHOW DATABASES;
```
![database](/assets/database-verify.png)


**Step-3: Deploy Backend**
```sh
# Deploy backend
kubectl apply -f backend.yaml

# SSH into pod
kubectl exec -it <backend-pod> -n dev -- sh

# Check DNS resolution
nslookup mysql-svc
```
![backend](/assets/backend-verify.png)


**Step-4: Deploy Frontend**
```sh
# Deploy frontend
kubectl apply -f frontend.yaml

# Verify locally
curl http://<NODE-IP>:30080
```
![frontend](/assets/frontend-verify.png)


**Step-5: Verify All Resources**
```sh
kubectl get all -n dev
kubectl get pvc -n dev
kubectl get configmap -n dev
kubectl get secret -n dev
```
![verification](/assets/output.png)


### 🧠 Key Learnings
- Namespace isolation
- Deployment vs StatefulSet
- Headless Service for databases
- NodePort vs ClusterIP
- Environment variable injection
  - `envFrom` (Database)
  - `env` + `valueFrom` (Backend)
- Persistent storage using PVC
- Kubernetes internal DNS
- Practical debugging techniques


### 🧪 Troubleshooting
| Problem           | Command                      |
| ----------------- | ---------------------------- |
| Pods not ready    | `kubectl describe pod <pod>` |
| DNS issue         | `nslookup service-name`      |
| Logs              | `kubectl logs <pod>`         |
| Env check         | `kubectl exec pod -- env`    |
| Service endpoints | `kubectl get endpoints`      |


### ⚠️ Known Limitations
- No real application logic
- Backend runs dummy process (`sleep`)
- No TLS between services
- No Ingress controller
- No autoscaling
