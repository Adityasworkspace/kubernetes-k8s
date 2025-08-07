
# Day 07 - Blue-Green Deployment Model in Kubernetes

## 🌈 What is Blue-Green Deployment?

**Blue-Green Deployment** is a **release management strategy** that reduces downtime and risk by running **two identical environments** (Blue & Green).

- **Blue Environment**: Current live/production version of the application.
- **Green Environment**: New version to be released.

### ✅ Why Use Blue-Green Deployment?

- Enables **zero-downtime deployments**
- Provides a **safe rollback mechanism**
- Allows **pre-production testing** in a production-like environment
- Reduces risk by switching traffic only when the new version is verified

---

## 📦 Steps to Perform Blue-Green Deployment

### 🔽 Step 0: Clone Git Repository

```bash
$ git clone https://github.com/ashokitschool/kubernetes_manifest_yml_files.git
```

---

### 📁 Step 1: Navigate into `blue-green` directory

```bash
$ cd kubernetes_manifest_yml_files/blue-green/
```

---

### 🔵 Step 2: Create Blue Pods Deployment

- The blue deployment should have the label: `pod-label: v1`

```bash
$ kubectl apply -f blue-deployment.yml
```

---

### 🌐 Step 3: Create Live Service to Expose Blue Pods

```bash
$ kubectl apply -f live-service.yml
```

---

### 🌍 Step 4: Access App using Live Service LoadBalancer URL

- You should see the **response from blue pods** in the browser.

```bash
$ kubectl get svc
```

---

### 🟢 Step 5: Create Green Pods Deployment

```bash
$ kubectl apply -f green-deployment.yml
```

---

### 📋 Step 6: Check All Running Pods

```bash
$ kubectl get pods -o wide
```

---

### 🧪 Step 7: Create Pre-Prod Service

- This exposes the **green deployment** separately for testing.

```bash
$ kubectl apply -f pre-prod-service.yml
```

---

### 🔍 Step 8: Access Green Pods via Pre-Prod Service

- You can test green version without impacting live traffic.

---

### ✅ Step 9: Make Green Pods Live

- Update **Live Service selector** to point to green pods (v2).
- This can be done by modifying the selector in the live-service YAML and reapplying.

```bash
# Edit the live service to switch to green
$ kubectl edit svc live-service
```

---

### 🧪 Step 10: Access the Live Service

- The **green pods** should now be serving live traffic.

```bash
$ kubectl get svc
```

---

## 🎯 Benefits Recap

- Seamless switch from old to new version
- Quick rollback by switching back to old pods
- Clean and controlled testing of new deployments

