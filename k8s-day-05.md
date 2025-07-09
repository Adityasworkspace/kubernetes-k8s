

# Day 05 - ReplicaSet and Service in Kubernetes

## 🚫 Why Not Create Pod Directly?

- When we create a Pod directly using a **Pod manifest (`kind: Pod`)**, we **do not** get **self-healing** capability.
- If the pod is **damaged/crashed/deleted**, **Kubernetes will not recreate it**.
- If the pod is down, our **application becomes unavailable**.
- ❗ **Best Practice**: Avoid creating standalone Pods for deploying applications in Kubernetes.

## ✅ What to Use Instead?

To manage Pods efficiently, we should use **Kubernetes resources** that handle the **Pod life cycle**:

### Available Resources:
1. `ReplicationController` (📦 **Outdated**)
2. `ReplicaSet` ✅
3. `Deployment` ✅
4. `DaemonSet`
5. `StatefulSet`

---

## 🌀 ReplicaSet

- **ReplicaSet** is a Kubernetes resource that **creates and manages Pods**.
- It ensures **desired number of Pod replicas** are always running.
- **Self-healing**: If a pod is deleted or crashes, ReplicaSet will automatically recreate it.
- Ensures **High Availability** of the application.
- Supports **scaling** (up/down) of Pods.

### Example: ReplicaSet YAML

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: javawebrs
spec:
  replicas: 2
  selector:
    matchLabels:
      app: javawebapp
  template:
    metadata:
      name: javawebpod
      labels:
        app: javawebapp
    spec:
      containers:
      - name: javawebcontainer
        image: ashokit/javawebapp
        ports:
        - containerPort: 8080
```

---

## 🌐 Service for Access

To expose our application, we use a **Service**:

### Example: LoadBalancer Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
  namespace: ashokit-ns
spec:
  type: LoadBalancer
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

---

## 🛠️ Useful Commands

```bash
# Apply the YAML files
$ kubectl apply -f <filename>.yml

# Get all Kubernetes resources
$ kubectl get all

# View ReplicaSets
$ kubectl get rs

# View Pods
$ kubectl get pods

# Delete a specific Pod (ReplicaSet will recreate it)
$ kubectl delete pod <pod-name>

# Scale up/down the ReplicaSet
$ kubectl scale rs javawebrs --replicas=3

# Delete the ReplicaSet (and all its Pods)
$ kubectl delete rs javawebrs
```

> 💡 Note: To delete pods managed by a ReplicaSet, you should delete the **ReplicaSet** itself. Otherwise, it will recreate the deleted pods automatically.

---

✅ With this approach, your application will always be available and fault-tolerant!

---

## 🚀 Kubernetes Deployment

> 📌 **Note**: In ReplicaSet, scaling (up/down) is a **manual process**.  
> ✅ **Kubernetes supports Auto Scaling when we use 'Deployment' to create Pods**.

### What is a Deployment?

- A **Deployment** is a Kubernetes resource used to manage and deploy applications.
- It is the **most recommended approach** to deploy applications in Kubernetes.
- It handles the **entire Pod lifecycle** with additional advantages:

### ✅ Advantages of Deployment:

1. Zero Downtime during updates
2. Auto Scaling support
3. Rolling Updates & Rollbacks

---

### Deployment Strategies:

| Strategy       | Description                                       |
|----------------|---------------------------------------------------|
| `ReCreate`     | Deletes all existing Pods and creates new ones    |
| `RollingUpdate`| Deletes and creates Pods **one by one**           |

---

### Example: Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: javawebdeploy
spec:
  replicas: 2
  strategy:
    type: RollingUpdate
  selector:
    matchLabels:
      app: javawebapp
  template:
    metadata:
      name: javawebpod
      labels:
        app: javawebapp
    spec:
      containers:
      - name: javawebappcontainer
        image: ashokit/javawebapp
        ports:
        - containerPort: 8080
```

### Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebsvc
spec:
  type: LoadBalancer
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

---

### 🔧 Commands for Deployment

```bash
# Delete all resources
$ kubectl delete all --all

# Apply deployment and service
$ kubectl apply -f <filename>.yml

# Get all resources
$ kubectl get all

# Scale the deployment
$ kubectl scale deployment javawebdeploy --replicas=4
$ kubectl scale deployment javawebdeploy --replicas=3
```

> 🌐 **Access the application using the Load Balancer URL.**

---

## 📈 AutoScaling in Kubernetes

### What is AutoScaling?

- AutoScaling dynamically **increases or decreases resources** based on demand.
- Kubernetes supports two types:
  1. **Horizontal Scaling** — Adds/removes Pod instances.
  2. **Vertical Scaling** — Increases the resource capacity of a single Pod (rarely used).

---

### 🔁 Horizontal Pod Autoscaler (HPA)

- HPA automatically adjusts the number of Pod replicas based on **CPU or Memory** usage.
- It relies on a **Metrics Server** to monitor and collect real-time resource usage.

### Metrics Server:

- A lightweight application that collects metrics (CPU/RAM) from Pods and Nodes.

### Commands:

```bash
# Check metrics of nodes
$ kubectl top nodes

# Check metrics of pods
$ kubectl top pods
```

> ⚠️ **HPA requires Metrics Server to be installed and running.**
