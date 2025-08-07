
# Day 06 - Horizontal Pod Autoscaler (HPA) and Metrics Server in Kubernetes

## 📌 Note

> By default, the **Metrics Server** is **not available** in a Kubernetes cluster.

🎥 **HPA Reference Video**: [Watch here](https://www.youtube.com/watch?si=-0fbmX91-irfAeAW&v=c-tsJrcB50I&feature=youtu.be)

---

## 🧩 Step 1: Install Metrics Server

1. **Clone the GitHub repository**:

```bash
$ git clone https://github.com/ashokitschool/k8s_metrics_server
```

2. **Navigate to the correct directory**:

```bash
$ cd k8s_metrics_server
$ ls deploy/1.8+/
```

3. **Apply the manifest files**:

```bash
$ kubectl apply -f deploy/1.8+/
```

> This will create necessary components like service account, roles, and role bindings.

4. **Verify the Metric Server is running**:

```bash
$ kubectl get all -n kube-system
```

5. **Use Metric Server to monitor resources**:

```bash
# Top-level metrics for nodes
$ kubectl top nodes

# Top-level metrics for pods
$ kubectl top pods
```

> ⚠️ Metrics Server is installed in the `kube-system` namespace.

---

## 🚀 Step 2: Deploy a Sample Application

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpa-demo-deployment
spec:
  selector:
    matchLabels:
      run: hpa-demo-deployment
  replicas: 1
  template:
    metadata:
      labels:
        run: hpa-demo-deployment
    spec:
      containers:
      - name: hpa-demo-deployment
        image: k8s.gcr.io/hpa-example
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
```

---

## 🌐 Step 3: Create a Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: hpa-demo-deployment
  labels:
    run: hpa-demo-deployment
spec:
  ports:
  - port: 80
  selector:
    run: hpa-demo-deployment
```

---

## 📈 Step 4: Create the HPA

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-demo-deployment
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-demo-deployment
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

---

## 🔥 Step 5: Increase the Load

```bash
# Check the HPA
$ kubectl get hpa

# Run a load generator
$ kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://hpa-demo-deployment; done"
```

> In another terminal or shell, monitor the HPA using:

```bash
$ kubectl get hpa -w

$ kubectl describe deploy hpa-demo-deployment

$ kubectl get hpa

$ kubectl get events

$ kubectl top pods
```

---

✅ With these steps, you have successfully implemented **HPA with Metrics Server** to autoscale your application based on CPU utilization.
