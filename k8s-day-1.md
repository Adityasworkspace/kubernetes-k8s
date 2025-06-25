<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/3/39/Kubernetes_logo_without_workmark.svg" width="150" alt="Kubernetes Logo"/>
</p>
# ☸️ Kubernetes - Day 1

---

## 🧠 What is Kubernetes?

- Kubernetes (K8s) is a **free & open-source** container orchestration platform.
- Originally developed by **Google**.
- Written in the **Go programming language**.
- Provides advanced orchestration to manage containers:
  - Start / Stop / Delete
  - Auto Scaling
  - Load Balancing
  - Self Healing

---

## ⚙️ Kubernetes vs Docker

| Feature | Docker | Kubernetes |
|--------|--------|------------|
| Purpose | Containerization Platform | Orchestration Platform |
| Use | Package and run apps in containers | Manage and coordinate containerized applications |
| Description | Packages app code + dependencies | Manages containers across multiple hosts |

> 📝 **Note:**  
> - *Containerization* = Packaging app & dependencies as a single unit (via Docker)  
> - *Orchestration* = Managing containers at scale (via Kubernetes)

---

## 🚀 Kubernetes Advantages

1. **Auto Scaling** – Automatically scales container count based on demand.
2. **Load Balancing** – Distributes traffic across available containers.
3. **Self Healing** – Automatically replaces crashed containers.

---

## 🧱 Kubernetes Architecture

- Kubernetes uses a **cluster architecture**.
- A **cluster** consists of:
  - **Control Plane (Master Node)**
  - One or more **Worker Nodes**

---

## 🏗️ Kubernetes Cluster Components

### 🖥️ 1. Control Plane (Master Node)

- **API Server** – Entry point for all cluster commands (via `kubectl`)
- **Scheduler** – Assigns tasks (pods) to worker nodes
- **Controller Manager** – Ensures desired state of the system
- **ETCD** – Key-value store acting as the cluster’s internal database

### ⚙️ 2. Worker Node

- **Kubelet** – Agent that runs on each node, communicates with master
- **Kube Proxy** – Handles networking and communication across the cluster
- **Docker Runtime** – Runs the actual containers
- **POD** – Basic deployable unit in Kubernetes
- **Container** – Application instance running inside a Pod

---

## 🔄 Kubernetes Workflow Overview

1. Developer uses `kubectl` to submit deployment request.
2. **API Server** receives the request and stores it in **ETCD**.
3. **Scheduler** reads pending tasks from ETCD.
4. Scheduler selects a Worker Node using **Kubelet**.
5. **Docker Engine** on that node creates a **Container** inside a **Pod**.
6. **Kube Proxy** ensures proper network routing.
7. **Controller Manager** monitors and ensures everything works as intended.

> 📌 In Kubernetes, **everything is created as a POD**.  
> A **Pod** is the smallest unit of deployment.

---

📅 **Day 1 of Kubernetes Completed**

📘 Coming Next: YAML Files, Pod Definitions, Deployments, and Services.
