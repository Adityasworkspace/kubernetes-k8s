
# Day 10 – ConfigMap & Secrets in Kubernetes  

## Environments in Application Lifecycle  
For every application, multiple environments will be available for testing purposes:  

- **DEV** → Development team uses for code integration testing  
- **SIT** → Testing team uses for functional testing  
- **UAT** → Client-side team uses for acceptance testing  
- **PILOT** → Pre-production testing  
- **PROD** → Final deployment (live environment)  

👉 Each environment has different properties, such as:  
1. Database properties  
2. SMTP properties  
3. Kafka properties  
4. Redis properties  

---

## Why Not Hardcode Properties?  
- We **shouldn’t hardcode** environment-specific properties in the source code.  
- Applications should be **loosely coupled** so they can be deployed in any environment without modifying the Docker image.  
- **Solution** → Use **ConfigMaps & Secrets**  

- **ConfigMap** → Stores **non-confidential** data in key-value format  
- **Secret** → Stores **confidential** data (e.g., passwords) in base64 encoded format  

✅ This approach makes Docker images **portable** across environments.  

---

## Example – Application Properties  

### Hardcoded Properties  
```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://mysqldb:3306/sbms
    username: root
    password: root
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
````

### With Environment Variables

```yaml
spring:
  datasource:
    driver-class-name: ${DB_DRIVER:com.mysql.cj.jdbc.Driver}
    url: ${DB_URL:jdbc:mysql://mysqldb:3306/sbms}
    username: ${DB_USERNAME:root}
    password: ${DB_PASSWORD:root}
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

---

## ConfigMap Manifest

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: ashokit-config-map
  labels:
    storage: ashokit-db-config-map
data:
  DB_DRIVER: com.mysql.cj.jdbc.Driver
  DB_URL: jdbc:mysql://mysqldb:3306/sbms
  DB_USERNAME: root
```

---

## Secret Manifest

👉 Encode values with Base64 → [https://www.base64encode.org/](https://www.base64encode.org/)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: ashokit-secret
  labels:
    storage: ashokit-db-secret
data:
  DB_PASSWORD: cm9vdA==   # "root" in base64
type: Opaque
```

---

## Reading Data from ConfigMap

```yaml
- name: DB_DRIVER
  valueFrom:
    configMapKeyRef:
      name: ashokit-config-map
      key: DB_DRIVER
```

---

## Reading Data from Secret

```yaml
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: ashokit-secret
      key: DB_PASSWORD
```

---

📌 With ConfigMaps and Secrets, applications become environment-independent and easily portable across Kubernetes clusters.

```

