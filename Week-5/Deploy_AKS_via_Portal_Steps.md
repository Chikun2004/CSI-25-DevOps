
#  Deploy AKS Cluster via Azure Portal – Step-by-Step Guide

##  Prerequisites
- An active Azure Subscription
- Azure account with sufficient permissions to create resources
- Azure AD users (for role assignments)

---

##  Step 1: Login to Azure Portal
Go to: [https://portal.azure.com](https://portal.azure.com)

---

##  Step 2: Create AKS Cluster

1. In the search bar, type **"Kubernetes services"** and click it.
2. Click **"Create"** > **"Add Kubernetes cluster"**.
3. **Basics Tab**:
   - **Subscription**: Choose your subscription.
   - **Resource Group**: Create a new or use existing.
   - **Cluster name**: `aks-cluster-demo`
   - **Region**: Choose nearest region (e.g., East US)
   - **Kubernetes version**: Select default or latest stable
   - **Node Size**: Standard_DS2_v2 or smaller
   - **Node count**: Start with 1 or 2

4. **Node Pools Tab**: Keep default settings or configure as needed.

5. **Authentication Tab**:
   - Select **System-assigned managed identity**
   - Enable **Azure AD integration** if needed

6. **Networking Tab**: Choose default networking (Azure CNI or Kubenet)

7. **Monitoring Tab**: Enable container insights

8. **Tags Tab**: (Optional) Add metadata

9. **Review + Create**: Click **Create**

⏳ Deployment may take ~5–10 minutes.

---

##  Step 3: Access AKS Cluster Dashboard

1. Go to the AKS resource
2. Click **"Connect"** and follow Azure CLI instructions to connect

```bash
az aks install-cli
az aks get-credentials --resource-group <your-rg> --name aks-cluster-demo
```

3. Use `kubectl` to verify:
```bash
kubectl get nodes
```

4. To access the Kubernetes dashboard:
```bash
az aks browse --resource-group <your-rg> --name aks-cluster-demo
```

Dashboard available at `http://127.0.0.1:8001`

---

##  Step 4: Create Roles for Multiple Users

### Step 4.1: Create Azure AD Users
- In **Azure Active Directory**, go to **Users** > **New User**
- Add multiple users (or guest users if needed)

### Step 4.2: Create Kubernetes Role (YAML)

Example: `read-only-role.yaml`

```yaml
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: default
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

### Apply Role
```bash
kubectl apply -f read-only-role.yaml
```

---

### Step 4.3: Create RoleBinding for Users

Example: `role-binding.yaml`

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods-binding
  namespace: default
subjects:
- kind: User
  name: "<Azure_AD_User_1>" 
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

>  Repeat this step for each user or use `Group` kind to bind for multiple users via Azure AD Group.

```bash
kubectl apply -f role-binding.yaml
```

---

##  Step 5: Verify Role Access

- Login as the assigned user
- Try accessing pods:
```bash
kubectl get pods --namespace default
```
- If set up correctly, access is limited as per defined role.

---

##  You now have:
- A working AKS cluster via Portal
- Dashboard access
- Roles and bindings for multiple users via Azure AD
