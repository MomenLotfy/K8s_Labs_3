# ☸️ Kubernetes Lab 3 — Multi-Tenancy with Namespaces

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)
![Namespaces](https://img.shields.io/badge/Namespaces-2-blue?style=for-the-badge)

---

## 📋 Objective

Demonstrate **namespace isolation** and **internal service communication** in a Kubernetes cluster by deploying isolated environments that simulate real-world multi-tenant setups.

---

## 🏗️ Architecture Overview

```
Kubernetes Cluster
├── Namespace: dev
│   ├── Pod: backend-pod
│   ├── Service: backend-svc
│   └── Pod: frontend-pod
│
└── Namespace: staging
    ├── Pod: backend-pod
    ├── Service: backend-svc
    └── Pod: frontend-pod
```

---

## 🌍 Environments

Two isolated namespaces were created to simulate separate deployment environments:

| Namespace  | Purpose                        |
|------------|--------------------------------|
| `dev`      | Active development environment |
| `staging`  | Pre-production testing environment |

Each environment contains:
- ✅ **backend-pod** — Backend application pod
- ✅ **backend-svc** — Internal ClusterIP service for backend
- ✅ **frontend-pod** — Frontend application pod

---

## 🚀 Deployment

### Create Namespaces

```bash
kubectl create namespace dev
kubectl create namespace staging
```

### Apply Resources

```bash
# Deploy to dev
kubectl apply -f backend-pod.yaml -n dev
kubectl apply -f backend-svc.yaml -n dev
kubectl apply -f frontend-pod.yaml -n dev

# Deploy to staging
kubectl apply -f backend-pod.yaml -n staging
kubectl apply -f backend-svc.yaml -n staging
kubectl apply -f frontend-pod.yaml -n staging
```

---

## ✅ Verification

### Check resources in `dev`

```bash
kubectl get pods,svc -n dev
```

### Check resources in `staging`

```bash
kubectl get pods,svc -n staging
```

---

## 🌐 Access the Application

Forward the frontend pod port to your local machine:

```bash
kubectl port-forward pod/frontend-pod 8082:80 -n dev
```

Then open your browser at:

```
http://localhost:8082
```

---

## 📸 Screenshots

### 1. Running Pods & Services — `dev` Namespace
![dev namespace](screenshot1.png)
> Output of `kubectl get pods,svc -n dev`

### 2. Running Pods & Services — `staging` Namespace
![staging namespace](screenshots2.png)
> Output of `kubectl get pods,svc -n staging`

### 3. Browser Output After Port-Forward

### 3. Browser Output After Port-Forward

![staging namespace](screenshots3.png)

> Frontend application accessible at `http://localhost:8082` via `kubectl port-forward`.


---

## 💡 Key Concepts Demonstrated

| Concept | Description |
|---|---|
| **Namespace Isolation** | Resources in `dev` are fully isolated from `staging` |
| **Internal Communication** | Pods communicate via ClusterIP services within the same namespace |
| **Port Forwarding** | Local access to cluster pods without exposing external endpoints |
| **Multi-Tenancy** | Multiple teams can share a single cluster with logical separation |

---

## 🔑 Useful Commands

```bash
# List all namespaces
kubectl get namespaces

# View all resources across all namespaces
kubectl get all --all-namespaces

# Describe a pod in dev
kubectl describe pod frontend-pod -n dev

# View pod logs
kubectl logs frontend-pod -n dev

# Delete all resources in a namespace
kubectl delete all --all -n dev
```

---

## 👤 Author

**Moamen** — DevOps Engineer  
*Lab completed as part of Kubernetes & DevSecOps learning path*

---

> 📝 **Note:** Place your screenshots in a `screenshots/` folder at the root of this repo and name them `dev-namespace.png`, `staging-namespace.png`, and `browser-output.png` to match the image references above.
