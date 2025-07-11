# CI/CD Pipeline to Build and Push Docker Image to ACR and Deploy to AKS

## Objective
Automate the process of building a Docker image, pushing it to Azure Container Registry (ACR), and deploying the updated container to Azure Kubernetes Service (AKS) using Azure DevOps CI/CD pipelines.

---

## Prerequisites
- Azure DevOps Project with access to Pipelines.
- Azure Container Registry (ACR) created (e.g., `myregistry.azurecr.io`).
- Azure Kubernetes Service (AKS) cluster set up.
- Kubernetes manifest (YAML) files for deployment.
- Service connections:
  - ACR service connection
  - AKS service connection (via Azure Resource Manager)

---

## Step 1: Prepare Your Source Code Repository
1. Ensure your repo contains:
   - `Dockerfile`
   - `deployment.yaml`, `service.yaml` (or `k8s` folder with manifests)
   - Application source code

---

## Step 2: Create Azure Resource Manager Service Connections

### A. ACR
1. Go to **Project Settings > Service connections**.
2. Click **New service connection > Docker Registry**.
3. Choose **Azure Container Registry**.
4. Select your Azure subscription and ACR.
5. Name the connection (e.g., `acr-connection`).
6. Grant access to all pipelines if needed.

### B. AKS
1. Click **New service connection > Azure Resource Manager**.
2. Choose **Service principal (automatic)**.
3. Select your subscription and AKS resource group.
4. Name it (e.g., `aks-connection`).
5. Verify and save.

---

## Step 3: Create the CI/CD Pipeline (YAML)

Create a file called `azure-pipelines.yml` in the root of your repo:

```yaml
trigger:
- main

variables:
  imageName: myapp

stages:
- stage: Build
  displayName: Build and Push Image
  jobs:
  - job: Build
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Docker@2
      displayName: Build and Push Docker Image
      inputs:
        containerRegistry: 'acr-connection'
        repository: '$(imageName)'
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
        tags: |
          $(Build.BuildId)

- stage: Deploy
  displayName: Deploy to AKS
  dependsOn: Build
  jobs:
  - job: Deploy
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Kubernetes@1
      displayName: Deploy to AKS
      inputs:
        connectionType: 'Azure Resource Manager'
        azureSubscription: 'aks-connection'
        azureResourceGroup: '<your-resource-group>'
        kubernetesCluster: '<your-aks-cluster>'
        namespace: 'default'
        command: apply
        useConfigurationFile: true
        configuration: |
          k8s/deployment.yaml
          k8s/service.yaml

Step 4: Modify Kubernetes Manifests
In k8s/deployment.yaml, set the image to use the ACR image and tag:
containers:
- name: myapp
  image: myregistry.azurecr.io/myapp:$(Build.BuildId)
  ports:
  - containerPort: 80
Ensure imagePullSecrets are configured if ACR is private.

Step 5: Commit and Push Code
Push all files (azure-pipelines.yml, Dockerfile, source, k8s manifests) to your main branch.

Azure DevOps will automatically trigger the pipeline.

Step 6: Monitor Pipeline Execution
Go to Pipelines > Pipelines in Azure DevOps.

Open the latest run.

Monitor:

Build Stage: Docker image built and pushed to ACR.

Deploy Stage: Manifests applied to AKS.

Step 7: Verify Deployment on AKS
Run the following to verify:
kubectl get pods
kubectl get svc

You should see your pod running and service exposed (based on your service.yaml).

Best Practices
Tag images using Git SHA or Build ID for traceability.

Use Helm charts for complex deployments.

Secure ACR with managed identities.

Use separate namespaces for dev, staging, and production.

References:
Azure DevOps Pipelines

Deploy to Kubernetes using Azure Pipelines

YouTube: Azure DevOps CI/CD with Docker & AKS