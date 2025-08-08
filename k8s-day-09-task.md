

# ![K8s Logo](https://upload.wikimedia.org/wikipedia/commons/3/39/Kubernetes_logo_without_workmark.svg)  
# Day 09 – HELM, Prometheus & Grafana in Kubernetes

---

## What is HELM?

- **HELM** is a **package manager** used to install required software in a Kubernetes cluster.
- HELM uses **charts** to install packages.
- **Chart** = Collection of configuration files (Manifest YAMLs).

---

## HELM Charts

Using HELM charts, we can easily install tools such as:

- **Prometheus Server**
- **Grafana Server**

---
Got it — you’re talking about adding the **prerequisite setup steps** before even touching the AWS Console for EKS.
Here’s a short, crisp version you can drop into your `.md` file before the Helm installation part:

---

## **Prerequisites – Setup Before Creating EKS Cluster**

Before creating the EKS cluster, install and configure the required tools on your local machine:

1. **Install AWS CLI** → [Download](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) 
![WhatsApp Image 2025-08-07 at 20 47 13_17105bea](https://github.com/user-attachments/assets/2d74d8f7-f011-450b-83f7-13203d732621)

   
2. **Install kubectl** (Kubernetes CLI) → [Guide](https://kubernetes.io/docs/tasks/tools/)
![WhatsApp Image 2025-08-07 at 20 48 54_e74caf56](https://github.com/user-attachments/assets/74eefec2-2116-4627-9109-d2ff9ce09556)

3. **Install eksctl** (EKS management tool) → [Guide](https://eksctl.io/introduction/#installation)
![WhatsApp Image 2025-08-07 at 20 48 54_e74caf56](https://github.com/user-attachments/assets/a2bc6a50-c8a3-427d-85f3-e421b85bffe8)

4. **Set Up AWS IAM Role**

   * Login to **AWS Console → IAM → Roles → Create Role**
   * Select **AWS Service → EKS**
   * Attach **AdministratorAccess** policy (for full permissions during setup)
   * Assign the role to your AWS user.
Suucessfully created AWS Eks Cluster 
![WhatsApp Image 2025-08-07 at 20 49 32_8ec92269](https://github.com/user-attachments/assets/aa89b6c8-f0ce-4c71-b680-e49a9a3da0b0)

✅ Once these tools are installed and IAM permissions are set, proceed to create the EKS cluster from the AWS Console.

---




## Installing HELM

```bash
# Download Helm installation script
$ curl -fsSl -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3

# Give execute permissions
$ chmod 700 get_helm.sh

# Run installation script
$ ./get_helm.sh

# Verify installation
$ helm
````

---

## Metrics Server Installation (via HELM)

```bash
# Check if metrics server is available
$ kubectl top pods
$ kubectl top nodes

# Check existing helm repos
$ helm repo ls
```
![WhatsApp Image 2025-08-07 at 20 50 57_bd8da71d](https://github.com/user-attachments/assets/a1a434bb-56a3-4f57-856e-6c0de3e94829)

```bash
# Add the metrics-server repo
$ helm repo add metrics-server https://kubernetes-sigs.github.io/metrics-server/
```
<img width="1751" height="618" alt="image" src="https://github.com/user-attachments/assets/06435580-d7e0-4054-8f6d-6504b22886c8" />


# Install the metrics-server chart
$ helm upgrade --install metrics-server metrics-server/metrics-server
```

---

## Kubernetes Monitoring Tools

We can monitor our Kubernetes cluster using:

1. **Prometheus** – Open-source monitoring and alerting toolkit; stores metrics as time series data.
2. **Grafana** – Visualization and monitoring tool providing dashboards, charts, and alerts.

> **Note:** Grafana connects to Prometheus as its data source.

---

## Deploying Prometheus & Grafana using HELM

### Step 1 – Add Helm Repositories

```bash
# Add stable repository
$ helm repo add stable https://charts.helm.sh/stable

# Add prometheus community repository
$ helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

# Update helm repositories
$ helm repo update
```

---

### Step 2 – Install Prometheus & Grafana

```bash
# Install Prometheus & Grafana stack
$ helm install stable prometheus-community/kube-prometheus-stack

# Get all pods
$ kubectl get pods
```

You should see Prometheus & Grafana pods running.

---

### Step 3 – Check Services

```bash
# List services
$ kubectl get svc
```
![WhatsApp Image 2025-08-07 at 20 52 05_ead2fde4](https://github.com/user-attachments/assets/044c831a-08ae-4e04-b104-ce1b8593e679)

By default, both Prometheus and Grafana services are **ClusterIP**.
To access them externally, change to **LoadBalancer**:

```bash
# Edit Prometheus Service
$ kubectl edit svc stable-kube-prometheus-sta-prometheus

# Edit Grafana Service
$ kubectl edit svc stable-grafana
```

---

### Step 4 – Verify Services

```bash
$ kubectl get svc
```
Successfully access the Prometheus enabling LBR SG group 9090 Port
![WhatsApp Image 2025-08-07 at 21 05 51_51c9bfb9](https://github.com/user-attachments/assets/5bcbdd43-89b3-4b79-b735-2ba4d0fe6486)

---

## Accessing Prometheus & Grafana

* **Prometheus:**
  `http://<LBR-DNS>:9090/`

* **Grafana:**
  `http://<LBR-DNS>/`

**Grafana Login Credentials:**

```
Username: admin
Password: prom-operator
```
![WhatsApp Image 2025-08-07 at 20 58 26_f653c9fd](https://github.com/user-attachments/assets/ce9cc510-4ef4-4dee-ad5a-8b1f70b6d22d)
![WhatsApp Image 2025-08-07 at 20 59 22_826c36de](https://github.com/user-attachments/assets/e91f777f-e26e-45f5-b0ba-2abcfd9da7d4)
![WhatsApp Image 2025-08-07 at 21 01 27_a89a847c](https://github.com/user-attachments/assets/4568b207-9976-4aaf-928f-91fe746df2b2)
![WhatsApp Image 2025-08-07 at 21 04 42_0eb583ea](https://github.com/user-attachments/assets/8f2a73d0-a376-4b39-89ee-3ceeab374ada)

![WhatsApp Image 2025-08-07 at 21 05 18_b1a5ab21](https://github.com/user-attachments/assets/fd24fb36-5f76-4803-a2e7-dd2a6e6c5c64)


---

Once logged into Grafana, you can monitor the Kubernetes cluster using dashboards and visual charts.

---

