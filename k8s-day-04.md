# 🚀 Kubernetes - Day 4

## 🟦 K8s NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: NodePort
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
````

### 🔹 What is NodePort?

* If we **don’t specify** `nodePort` in the service YAML, Kubernetes will assign a **random port between `30000–32767`**.
* To use a **specific port**, we can define it explicitly:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: NodePort
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30070
```

### 🧪 Commands

Here's your Kubernetes Day 4 documentation converted into a well-formatted **Markdown (`.md`)** file for your GitHub repo:

---

````markdown
# 🚀 Kubernetes - Day 4

## 🟦 K8s NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: NodePort
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
````

### 🔹 What is NodePort?

* If we **don’t specify** `nodePort` in the service YAML, Kubernetes will assign a **random port between `30000–32767`**.
* To use a **specific port**, we can define it explicitly:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: NodePort
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30070
```

### 🧪 Commands

: To execute k8s manifest yml
```bash
$ kubectl apply -f <yml>
```
: To check pods available
```bash
$ kubectl get pods
```
: To check k8s service
```bash
$ kubectl get service
```
: To check in which worker node, POD is running
```bash
$ kubectl get pods -o wide
```

📌 **Note**: Ensure the `nodePort` (e.g., `30070`) is allowed in the **inbound rules** of the worker node's **security group**.

🌐 **Access URL**:

```
http://<node-public-ip>:<nodePort>/java-web-app
```

---

## 🟨 K8s ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: ClusterIP
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

✅ `ClusterIP` exposes the service **internally within the cluster** using a static internal IP.

---

## 🟧 K8s Namespaces

### 📌 Why Use Namespaces?

Namespaces are used to **logically group resources** in Kubernetes:

* `frontend-app-pods` → Namespace A
* `backend-app-pods` → Namespace B
* `database-pods` → Namespace C

✅ Namespaces provide **isolation**.

🗑️ Deleting a namespace removes **all resources** inside it.

---

### 🔍 View Namespaces and Resources
 # list all namespaces
```bash
$ kubectl get ns
```
# get pods in a specific namespace
```bash                    
$ kubectl get pods -n kube-system   
```

> ⚠️ Default namespace is used if not explicitly mentioned.

---

### ✅ Create Namespaces

#### 🔸 Method 1: Command

```bash
$ kubectl create ns <ns>
```

#### 🔸 Method 2: YAML

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: <ns>
```

```bash
$ kubectl apply -f <yml>
```

---

### ❌ Delete Namespace

```bash
$ kubectl delete ns <namespace-name>
```

---

## 🛠️ Namespace with Pod & Service YAML

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: <ns>

---
apiVersion: v1
kind: Pod
metadata:
  name: javawebapppod
  namespace: <ns>
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
  namespace: <ns>
spec:
  type: LoadBalancer
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

### 🧪 Commands to Work with Namespace

```bash
$ kubectl apply -f <yml>
$ kubectl get ns
$ kubectl get pods -n ashokit-ns
$ kubectl get service -n ashokit-ns
$ kubectl get all -n ashokit-ns
$ kubectl delete ns ashokit-ns
```

---

🗓️ **Day 4 Summary**

* ✅ Learned NodePort, ClusterIP, and Namespace usage.
* 🛠️ Practiced with YAML manifests and `kubectl` commands.
* 🚀 Deployed applications in isolated namespaces with internal/external access configurations.

```

---

Let me know if you'd like me to create a `README.md` file with this content and include it in a sample folder structure for your Day 4 GitHub repo.
```

$ kubectl apply -f <yml>
$ kubectl get pods
$ kubectl get service
$ kubectl get pods -o wide
```

📌 **Note**: Ensure the `nodePort` (e.g., `30070`) is allowed in the **inbound rules** of the worker node's **security group**.

🌐 **Access URL**:

```
http://<node-public-ip>:<nodePort>/java-web-app
```

---

## 🟨 K8s ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: javawebappsvc
spec:
  type: ClusterIP
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
```

✅ `ClusterIP` exposes the service **internally within the cluster** using a static internal IP.

---

## 🟧 K8s Namespaces

### 📌 Why Use Namespaces?

Namespaces are used to **logically group resources** in Kubernetes:

* `frontend-app-pods` → Namespace A
* `backend-app-pods` → Namespace B
* `database-pods` → Namespace C

✅ Namespaces provide **isolation**.

🗑️ Deleting a namespace removes **all resources** inside it.

---

### 🔍 View Namespaces and Resources
 # list all namespaces
```bash
$ kubectl get ns 
``` 
 # get pods in a specific namespace  
```bash                 
$ kubectl get pods -n kube-system   
```

> ⚠️ Default namespace is used if not explicitly mentioned.

---

### ✅ Create Namespaces

#### 🔸 Method 1: Command

```bash
$ kubectl create ns <ns>
```

#### 🔸 Method 2: YAML

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: <ns>
```

```bash
$ kubectl apply -f <yml>
```

---

### ❌ Delete Namespace

```bash
$ kubectl delete ns <namespace-name>
```

---

## 🛠️ Namespace with Pod & Service YAML

```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: <ns>

---
apiVersion: v1
kind: Pod
metadata:
  name: javawebapppod
  namespace: <ns>
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
  namespace: <ns>
spec:
  type: LoadBalancer
  selector:
    app: javawebapp
  ports:
    - port: 80
      targetPort: 8080
...
```

### 🧪 Commands to Work with Namespace

```bash
$ kubectl apply -f <yml>
$ kubectl get ns
$ kubectl get pods -n <ns>
$ kubectl get service -n <ns>
$ kubectl get all -n <ns>
$ kubectl delete ns <ns>
```

---

🗓️ **Day 4 Summary**

* ✅ Learned NodePort, ClusterIP, and Namespace usage.
* 🛠️ Practiced with YAML manifests and `kubectl` commands.
* 🚀 Deployed applications in isolated namespaces with internal/external access configurations.

```

