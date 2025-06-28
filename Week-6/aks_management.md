
# Managing Kubernetes with Azure Kubernetes Service (AKS)

##  Introduction
Azure Kubernetes Service (AKS) simplifies deploying a managed Kubernetes cluster in Azure by offloading much of the complexity and operational overhead.

---

## 1.  Creating an AKS Cluster

###  Prerequisites:
- Azure CLI installed (`az`)
- Logged into Azure (`az login`)
- Resource group created

###  Step-by-Step:

```bash
# Create a resource group
az group create --name myResourceGroup --location eastus

# Create AKS cluster
az aks create   --resource-group myResourceGroup   --name myAKSCluster   --node-count 3   --enable-addons monitoring   --generate-ssh-keys

# Get AKS credentials
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster

# Verify cluster access
kubectl get nodes
```

---

## 2.  Managing AKS Clusters

###  Common Management Tasks:

```bash
# View cluster details
az aks show --resource-group myResourceGroup --name myAKSCluster

# View Kubernetes context
kubectl config get-contexts

# Set context to AKS
kubectl config use-context <aks-context-name>
```

---

## 3.  Scaling AKS Clusters

###  Manually Scale Node Pool

```bash
az aks scale   --resource-group myResourceGroup   --name myAKSCluster   --node-count 5
```

###  Enable Cluster Autoscaler

```bash
az aks update   --resource-group myResourceGroup   --name myAKSCluster   --enable-cluster-autoscaler   --min-count 1   --max-count 5
```

---

## 4.  Upgrading AKS Clusters

###  Check for Available Upgrades

```bash
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster
```

###  Upgrade Cluster Version

```bash
az aks upgrade   --resource-group myResourceGroup   --name myAKSCluster   --kubernetes-version <new-version>
```

---

## 5.  Useful Add-ons and Features

- Monitoring with Azure Monitor
- Azure Active Directory (AAD) integration
- Network policies
- Azure CNI/Calico networking

---

##  Best Practices

| Task | Best Practice |
|------|---------------|
| Node Scaling | Use autoscaler to manage cost |
| Cluster Upgrade | Always backup workloads |
| Monitoring | Enable built-in Azure Monitor |
| Security | Use RBAC and Azure AD integration |

---

##  Summary

- **AKS** simplifies Kubernetes on Azure.
- Use `az` CLI for lifecycle management.
- Automate scaling and stay updated with upgrades.

