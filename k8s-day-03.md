
# ☸️ Kubernetes - Day 3: Manifest YAMLs

---

## 📄 Kubernetes Manifest YAML Overview

> In Kubernetes, we use **Manifest YAML files** to define and deploy resources like Pods, Services, Deployments, etc.

---

## 🧱 Kubernetes Manifest YAML Syntax

```yaml
---
apiVersion: <version-number>
kind: <resource-type>
metadata:
  name: <name>
spec:
  <specifications>
```

🔧 Apply the Manifest YAML using:

```bash
kubectl apply -f <manifest-yml>
```

---

## 🚀 K8s POD Manifest YAML

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: javawebapppod
  labels:
    app: javawebapp
spec:
  containers:
    - name: javawebappcontainer
      image: ashokit/javawebapp
      ports:
        - containerPort: 8080
```

### Useful Commands

```bash
kubectl get pods
kubectl apply -f <manifest-yml>
kubectl get pods -o wide
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```

---

## 🌐 K8s Service Manifest YAML

> Services expose Pods to internal or external networks.

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: LoadBalancer
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

### Commands

```bash
kubectl get service
kubectl apply -f <service-manifest-yml>
```

🔗 Access application via **Load Balancer DNS URL**:
```
http://<load-balancer-dns>/java-web-app/
```

---

## 🔄 POD & Service in a Single YAML

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: javawebapppod
  labels:
    app: javawebapp
spec:
  containers:
    - name: javawebappcontainer
      image: ashokit/javawebapp
      ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: LoadBalancer
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

### Final Commands

```bash
kubectl apply -f <manifest-yml>
kubectl get pods
kubectl get service
kubectl delete all --all
```

---
