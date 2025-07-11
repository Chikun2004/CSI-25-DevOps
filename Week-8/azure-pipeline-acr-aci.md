#  Azure DevOps CI/CD Pipeline: Build & Push Docker Image to ACR and Deploy to ACI

This guide demonstrates how to create a CI/CD pipeline using Azure DevOps that builds a Docker image, pushes it to Azure Container Registry (ACR), and deploys it to Azure Container Instance (ACI).

---

##  Prerequisites

1. Azure Subscription
2. Azure CLI installed
3. Azure DevOps Project
4. Dockerfile in the root of your repo
5. A sample app (example: a Node.js "Hello World" app)

---

##  Sample App Structure

my-app/
│
├── Dockerfile
├── app.js
└── package.json

app.js :-

**app.js:**

```js
const http = require('http');
const port = process.env.PORT || 80;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello from Azure Container Instance!\n');
});

server.listen(port, () => {
  console.log(`Server running on port ${port}`);
});

package.json:-

{
  "name": "azure-aci-demo",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  }
}

Docker-File: -

FROM node:18
WORKDIR /usr/src/app
COPY . .
RUN npm install
EXPOSE 80
CMD ["npm", "start"]

Step 1: Create Azure Resources

# Login
az login

# Create Resource Group
az group create --name rg-acr-aci --location eastus

# Create Azure Container Registry
az acr create --resource-group rg-acr-aci --name acrdemoregistry --sku Basic --admin-enabled true


Step 2: Get ACR Credentials

az acr credential show --name acrdemoregistry

--Save username and passwords[0].value for Azure DevOps pipeline secrets.


Step 3: Create Service Connection in Azure DevOps

Go to Azure DevOps → Project Settings → Service Connections

Create an Azure Resource Manager connection (Service Principal)

Scope to your subscription and rg-acr-aci resource group

Name it AzureConnection

Step 4: Create Pipeline Secrets

Go to Azure DevOps → Pipelines → Library → New Variable Group

Name: ACRSecrets

Add secrets:

ACR_USERNAME = your ACR username

ACR_PASSWORD = your ACR password

Save and authorize for use in pipelines

Step 5: Add Pipeline YAML (azure-pipelines.yml)

trigger:
  branches:
    include:
      - main

variables:
  imageName: 'azure-aci-demo'
  acrLoginServer: 'acrdemoregistry.azurecr.io'
  containerInstanceName: 'nodejsdemoaci'
  resourceGroupName: 'rg-acr-aci'
  dnsNameLabel: 'acihelloworlddemo'

stages:
- stage: Build
  jobs:
  - job: DockerBuildPush
    pool:
      vmImage: 'ubuntu-latest'
    variables:
      - group: ACRSecrets
    steps:
    - task: Docker@2
      displayName: Build and Push Docker image
      inputs:
        containerRegistry: 'AzureConnection'
        repository: '$(imageName)'
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
        tags: |
          latest

- stage: Deploy
  dependsOn: Build
  jobs:
  - job: ACI_Deploy
    pool:
      vmImage: 'ubuntu-latest'
    variables:
      - group: ACRSecrets
    steps:
    - task: AzureCLI@2
      inputs:
        azureSubscription: 'AzureConnection'
        scriptType: 'bash'
        scriptLocation: 'inlineScript'
        inlineScript: |
          az container delete --name $(containerInstanceName) --resource-group $(resourceGroupName) --yes || true
          az container create \
            --resource-group $(resourceGroupName) \
            --name $(containerInstanceName) \
            --image $(acrLoginServer)/$(imageName):latest \
            --registry-login-server $(acrLoginServer) \
            --registry-username $(ACR_USERNAME) \
            --registry-password $(ACR_PASSWORD) \
            --dns-name-label $(dnsNameLabel) \
            --ports 80

Step 6: Commit and Run Pipeline

Push your code including azure-pipelines.yml to the main branch.
Go to Azure DevOps → Pipelines → Run Pipeline.

