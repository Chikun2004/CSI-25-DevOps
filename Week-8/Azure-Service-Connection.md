# Create a Service Connection in Azure DevOps

## Objective
Set up a secure service connection in Azure DevOps to authenticate and integrate with external services such as Azure, DockerHub, GitHub, AWS, etc., for deploying and managing applications.

---

## Prerequisites
- You must be a Project Administrator or have **Manage service connections** permission.
- External credentials (e.g., Azure subscription, DockerHub token, AWS keys) must be available.
- Target Azure DevOps project must be accessible.

---

## Step 1: Open Azure DevOps Project
1. Go to [https://dev.azure.com](https://dev.azure.com).
2. Choose your organization and open the desired **Project**.

---

## Step 2: Navigate to Service Connections
1. From the left sidebar, go to **Project Settings**.
2. Under **Pipelines**, click on **Service connections**.

---

## Step 3: Create a New Service Connection
1. Click **+ New service connection**.
2. Select the type of connection based on your integration need. Examples include:
   - **Azure Resource Manager**
   - **Docker Registry**
   - **GitHub**
   - **AWS**
   - **Generic**
3. Click **Next** after selecting the service.

---

## Step 4: Configure the Selected Service Connection

### A. For Azure Resource Manager
1. Select **Service principal (automatic)** or **Service principal (manual)**.
2. Choose or enter:
   - Subscription
   - Scope level (e.g., Subscription/Resource Group)
   - Resource group name (if applicable)
3. Provide a **Service connection name** (e.g., `Azure-Prod-Connection`).
4. Optionally check **Grant access permission to all pipelines**.
5. Click **Save**.

### B. For Docker Registry
1. Choose the registry type (e.g., DockerHub, Others).
2. Enter:
   - Docker ID
   - Password/Token
   - Connection name
3. Optionally allow access to all pipelines.
4. Click **Save**.

### C. For AWS
1. Provide:
   - AWS Access Key ID
   - AWS Secret Access Key
   - Region
   - Connection name
2. Click **Save**.

> The steps may slightly differ based on the selected service connection type. Refer to official documentation for specific configuration.

---

## Step 5: Validate the Service Connection
1. After saving, the service connection will appear in the list.
2. Click on the connection to view details and verify connectivity status.
3. Use the **Verify** button (if available) to test the connection.

---

## Step 6: Use Service Connection in a Pipeline

### YAML Example (Azure RM)

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: AzureCLI@2
  inputs:
    azureSubscription: 'Azure-Prod-Connection'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      az group list

Replace Azure-Prod-Connection with the name of your actual service connection.

*** Best Practices:-
-Use RBAC: Assign least-privilege access on cloud platforms for the service principal.

-Use Managed Identities (when supported) for secure, keyless authentication.

-Restrict service connection access to only those pipelines or users that need it.

-Rotate credentials periodically to maintain security compliance.