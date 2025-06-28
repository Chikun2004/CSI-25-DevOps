# Kubernetes Controllers: RC, RS & Deployment

## 1. ReplicationController

**Purpose:** Maintain a fixed number of pods.  
**YAML:**
\`\`\`yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
\`\`\`
**Deploy:**
\`\`\`
kubectl apply -f rc.yaml
kubectl get rc
kubectl get pods
\`\`\`
**Advantages:**
- Simple and reliable.
- Ensures pod count.

**Disadvantages:**
- Simple selectors only.
- No rolling update or rollback.
- Deprecated by ReplicaSet.

---

## 2. ReplicaSet

**Purpose:** Maintain pod count with flexible selectors.  
**YAML:**
\`\`\`yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
\`\`\`
**Deploy:**
\`\`\`
kubectl apply -f rs.yaml
kubectl get rs
kubectl get pods
\`\`\`
**Advantages:**  
- Set-based selectors.  
- Auto-recovery.  
- Useful for static scaling.

**Disadvantages:**  
- No native rolling updates/rollback.  
- Rarely used standalone.

---

## 3. Deployment

**Purpose:** Manage ReplicaSets with updates & rollbacks.  
**YAML:**
\`\`\`yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
\`\`\`
**Deploy:**
\`\`\`
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl rollout status deployment/nginx-deployment
kubectl get rs
kubectl get pods --show-labels
\`\`\`
**Advantages:**  
- Rolling updates.  
- Rollbacks.  
- Version control.  
- Pausing & cleanup strategies.

**Disadvantages:**  
- Higher complexity.  
- More YAML overhead.

---

