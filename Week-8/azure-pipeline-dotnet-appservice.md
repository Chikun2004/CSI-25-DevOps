#  Azure DevOps CI/CD Pipeline for .NET Application to Azure App Service

This guide walks you through building and deploying a .NET Core Web App using Azure DevOps CI/CD pipelines to Azure App Service.

---

##  Sample Project Structure

dotnet-sample-app/
│
├── Controllers/
├── Views/
├── wwwroot/
├── dotnet-sample-app.csproj
└── azure-pipelines.yml


---

## 🧰 Prerequisites

- Azure Subscription
- Azure DevOps Project
- .NET Core Web App (ASP.NET Core MVC or API)
- Azure App Service (Web App) created
- Service connection in Azure DevOps

---

## 🌐 Step 1: Create Azure App Service (if not created)

```bash
# Login to Azure
az login

# Create resource group
az group create --name rg-dotnet-app --location eastus

# Create App Service Plan
az appservice plan create --name dotnetAppPlan --resource-group rg-dotnet-app --sku FREE

# Create Web App
az webapp create --name dotnet-web-demo --resource-group rg-dotnet-app --plan dotnetAppPlan --runtime "DOTNETCORE|8.0"

Step 2: Create Azure DevOps Service Connection

Go to Azure DevOps → Project Settings → Service Connections

Select Azure Resource Manager

Use automatic (Service Principal) method

Scope it to the subscription and resource group rg-dotnet-app

Name it AzureConnection

 Step 3: Sample .NET Web App 

 dotnet new mvc -n dotnet-sample-app
cd dotnet-sample-app
dotnet run

Step 4: Create azure-pipelines.yml File

trigger:
  branches:
    include:
      - main

variables:
  buildConfiguration: 'Release'
  webAppName: 'dotnet-web-demo'
  azureSubscription: 'AzureConnection'

pool:
  vmImage: 'windows-latest'

stages:
- stage: Build
  displayName: 'Build Stage'
  jobs:
  - job: Build
    displayName: 'Build Job'
    steps:
    - task: UseDotNet@2
      displayName: 'Install .NET SDK'
      inputs:
        packageType: 'sdk'
        version: '8.x'
        installationPath: $(Agent.ToolsDirectory)/dotnet

    - task: DotNetCoreCLI@2
      displayName: 'Restore NuGet Packages'
      inputs:
        command: 'restore'
        projects: '**/*.csproj'

    - task: DotNetCoreCLI@2
      displayName: 'Build Solution'
      inputs:
        command: 'build'
        projects: '**/*.csproj'
        arguments: '--configuration $(buildConfiguration)'

    - task: DotNetCoreCLI@2
      displayName: 'Publish Project'
      inputs:
        command: 'publish'
        publishWebProjects: true
        arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'
        zipAfterPublish: true

    - task: PublishBuildArtifacts@1
      displayName: 'Publish Artifact'
      inputs:
        PathtoPublish: '$(Build.ArtifactStagingDirectory)'
        ArtifactName: 'drop'

- stage: Deploy
  displayName: 'Deploy Stage'
  dependsOn: Build
  jobs:
  - deployment: DeployWebApp
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            displayName: 'Deploy to Azure Web App'
            inputs:
              azureSubscription: '$(azureSubscription)'
              appType: 'webApp'
              appName: '$(webAppName)'
              package: '$(Pipeline.Workspace)/drop/**/*.zip'

 Step 5: Commit and Push

 Push your .NET project and azure-pipelines.yml to the main branch of your Azure DevOps repo.

  Step 6: Run the Pipeline

  Go to Pipelines → New Pipeline

Choose your repo and select "YAML"

Azure DevOps will auto-detect azure-pipelines.yml

Click Run

