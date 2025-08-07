
---

# 📦 Kubernetes Day 08 – Dockerized Monitoring with Helm, Prometheus & Grafana

In this guide, we deploy **Prometheus** and **Grafana** using **Docker Compose** for a local setup and also explore how to use **Helm** to install them in a Kubernetes cluster for production-grade monitoring.

---

## 🧠 What is Helm?

- Helm is a **package manager for Kubernetes**.
- It simplifies the deployment of applications using **charts**, which are collections of pre-configured Kubernetes resources.

---

## ⚙️ Helm Installation

```bash
# Download Helm installation script
$ curl -fsSl -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3

# Set permissions
$ chmod 700 get_helm.sh

# Run the script
$ ./get_helm.sh

# Verify Helm is installed
$ helm version
````

---

## 📊 Check if Metrics Server is Installed

```bash
$ kubectl top pods
$ kubectl top nodes
```

> If it’s not installed, use Helm to install it:

```bash
# Add metrics-server Helm repo
$ helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/

# Install metrics-server using Helm
$ helm upgrade --install metrics-server metrics-server/metrics-server
```

---

## 🐳 Dockerized Setup for Prometheus & Grafana (Local)

### 📁 Folder Structure

```
k8s-day08-monitoring/
│
├── docker/
│   ├── prometheus/
│   │   └── prometheus.yml
│   └── grafana/
│       └── provisioning/
│           └── datasources/
│               └── datasource.yml
│
├── docker-compose.yml
├── README.md
```

---

### 📄 docker/prometheus/prometheus.yml

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

---

### 📄 docker/grafana/provisioning/datasources/datasource.yml

```yaml
apiVersion: 1

datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    orgId: 1
    url: http://prometheus:9090
    isDefault: true
    editable: true
```

---

### 📄 docker-compose.yml

```yaml
version: '3.7'

services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    volumes:
      - ./docker/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana
    container_name: grafana
    depends_on:
      - prometheus
    volumes:
      - ./docker/grafana/provisioning/:/etc/grafana/provisioning/
    ports:
      - "3000:3000"
```

---

### 🟢 Start Docker Containers

```bash
$ docker-compose up -d
```

* Access **Prometheus** at 👉 [http://localhost:9090](http://localhost:9090)
* Access **Grafana** at 👉 [http://localhost:3000](http://localhost:3000)

🔐 **Grafana Default Credentials:**

* **Username:** `admin`
* **Password:** `admin`

You will be prompted to change the password on first login.

---

## ☸️ Kubernetes Monitoring using Helm

### 🪣 Add Required Helm Repos

```bash
$ helm repo add stable https://charts.helm.sh/stable
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
$ helm repo update
```

---

### 📦 Install Prometheus & Grafana in Kubernetes

```bash
$ helm install stable prometheus-community/kube-prometheus-stack
```

---

### ✅ Verify Installation

```bash
# Get pods
$ kubectl get pods

# Get services
$ kubectl get svc
```

---

### 🌐 Expose Services Outside the Cluster

By default, services are **ClusterIP** (internal only). To access them externally:

```bash
# Edit Prometheus service
$ kubectl edit svc stable-kube-prometheus-sta-prometheus

# Change `type: ClusterIP` to `type: LoadBalancer`, then save

# Edit Grafana service
$ kubectl edit svc stable-grafana

# Again, change to `type: LoadBalancer`
```

Verify:

```bash
$ kubectl get svc
```

---

## 🔗 Access URLs in Kubernetes

* **Prometheus:** `http://<LoadBalancer-DNS>:9090/`
* **Grafana:** `http://<LoadBalancer-DNS>/`

🔐 **Grafana Login (K8s):**

* **Username:** `admin`
* **Password:** `prom-operator`

---

## 📈 What We Achieved

| Tool       | Purpose                          |
| ---------- | -------------------------------- |
| Helm       | Package management in Kubernetes |
| Prometheus | Cluster monitoring & alerting    |
| Grafana    | Metrics visualization dashboard  |

---

📘 **Day 08 Completed**
📁 **Topic:** Monitoring in K8s using Helm, Prometheus & Grafana
🧰 **Tools:** Helm · Docker · Prometheus · Grafana · Kubernetes

---




