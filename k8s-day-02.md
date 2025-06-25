<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/3/39/Kubernetes_logo_without_workmark.svg" width="120" alt="Kubernetes Logo"/>
</p>

# ☸️ Kubernetes - Day 2

---

## 🏗️ K8s Cluster Setup

### 1. Mini Kube
- Single node cluster
- Best suited for practice and local testing

### 2. Kubeadm Cluster
- Self-managed cluster
- User is responsible for entire setup and maintenance

### 3. Provider Managed Cluster
- Ready-to-use cluster managed by cloud providers
- Examples:
  - AWS EKS
  - Azure AKS
  - Google GKE

> ⚠️ These managed services are **chargeable**.

---

## 📦 Kubernetes Resources

1. Pods
2. Services (Cluster IP, NodePort, Load Balancer)
3. Namespaces
4. ReplicationController (RC)
5. ReplicaSet
6. Deployment
7. DaemonSet
8. StatefulSet
9. IngressController
10. HPA (Horizontal Pod Autoscaler)
11. Helm Charts
12. Monitoring (Prometheus & Grafana)
13. Logging Stack (EFK - Elasticsearch, Fluentd, Kibana)

---

## 🔹 What is a Pod?

- Pod is the **smallest deployable unit** in Kubernetes.
- Applications are deployed as Pods.
- A single app can run in **multiple Pods**.
- Pods are defined using a **YAML Manifest file**.
- The YAML contains the Docker image & config.

### 🚀 Features of Pods

- **Self-Healing** – Recreated if damaged/crashed.
- **Load Balancing** – K8s distributes load across multiple pods.
- **Auto Scaling** – K8s can scale pods based on load.

---

## 🌐 Kubernetes Services

Services are used to **expose Pods** within or outside the cluster.

### 1. Cluster IP

- Default type of service.
- Links multiple pods to a **single static internal IP**.
- Can only be accessed **within the cluster**.
- Pods get replaced → IPs change → use ClusterIP for consistency.

### 2. NodePort

- Used to expose Pods **outside the cluster**.
- Access app using **Worker Node’s public IP and port**.
- All traffic goes to a single node (can create load imbalance).

### 3. LoadBalancer

- Integrates with **Cloud Load Balancers** (e.g., AWS, Azure).
- Automatically distributes requests across all pods and worker nodes.

---

## 🧩 Kubernetes Namespaces

- Used to **group and isolate resources** within the cluster.

| Example | Namespace |
|---------|-----------|
| `frontend-app-pods` | `frontend-app-ns` |
| `backend-app-pods` | `backend-app-ns` |
| `database-pods` | `database-ns` |

---

📅 **Day 2 of Kubernetes Completed**  
📘 Coming Next: Writing YAMLs, Deployments, ReplicaSets, and Services
