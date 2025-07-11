#  Azure DevOps CI/CD Pipeline for React App Deployment to Azure VM

This guide outlines how to build a CI/CD pipeline for a React.js application that:
1. Builds the React app using Azure DevOps
2. Copies build files to an Azure VM via SSH for deployment

---

##  Prerequisites

- Azure DevOps Project
- Azure Virtual Machine (Ubuntu/Linux preferred)
- Public IP and Username/Password or SSH Key of VM
- React App with `npm build` support
- SSH enabled on VM (`port 22` open in NSG)
- `nginx` or `http-server` installed on VM (to serve the build)

---

## 🗂️ Sample React App Folder Structure

my-react-app/
├── public/
├── src/
├── package.json
├── .gitignore
├── azure-pipelines.yml


---

## 🌐 Step 1: Prepare Azure VM

SSH into your VM and run:

```bash
# Update and install nginx or Node http-server
sudo apt update
sudo apt install nginx -y

# OR install http-server for static deployment
sudo apt install nodejs npm -y
sudo npm install -g http-server

# Create app folder
mkdir -p ~/react-app


 Step 2: Setup SSH Keys or Password Authentication

 Option A: SSH Key-based
Generate a key (if not available):
ssh-keygen -t rsa -b 4096
Copy the public key to your VM:
ssh-copy-id username@<VM_PUBLIC_IP>
 Step 3: Create a Service Connection in Azure DevOps
Not required for direct VM deployment. We will use an SSH task instead.

 Step 4: Create azure-pipelines.yml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  vmHost: '<VM_PUBLIC_IP>'
  vmUser: '<VM_USERNAME>'
  vmTargetPath: '/home/<VM_USERNAME>/react-app'
  sshKey: 'vm-ssh-key'  # Stored in Azure DevOps Pipeline secrets

stages:
- stage: Build
  jobs:
  - job: BuildReact
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '18.x'
      displayName: 'Install Node.js'

    - script: |
        npm install
        npm run build
      displayName: 'Install Dependencies and Build React App'

    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: 'build'
        ArtifactName: 'react-build'

- stage: Deploy
  dependsOn: Build
  jobs:
  - job: DeployToVM
    steps:
    - download: current
      artifact: react-build

    - task: CopyFilesOverSSH@0
      inputs:
        sshEndpoint: 'AzureVMConnection'  # Define this service connection in project settings (SSH)
        sourceFolder: '$(Pipeline.Workspace)/react-build'
        targetFolder: '$(vmTargetPath)'
        cleanTargetFolder: true
        overwrite: true

    - task: SSH@0
      inputs:
        sshEndpoint: 'AzureVMConnection'
        runOptions: 'inline'
        inline: |
          cd $(vmTargetPath)
          npx http-server -p 3000
🔐 Step 5: Create SSH Service Connection in Azure DevOps
Go to Project Settings → Service Connections

Select SSH

Fill in:

Host: <VM_PUBLIC_IP>

Port: 22

Username: <VM_USERNAME>

Authentication:

Password OR Private Key

Name it: AzureVMConnection

 Step 6: Trigger the Pipeline
Push code to the main branch

Azure DevOps Pipeline will:

Build the React app

Upload build to the VM via SSH

Start the server using http-server

 Final Output
Your app will be available at:
http://<VM_PUBLIC_IP>:3000
Make sure port 3000 is allowed in your VM’s Network Security Group (NSG).

 Optional: Setup nginx to Auto-Serve React App
sudo cp -r ~/react-app/build/* /var/www/html/
sudo systemctl restart nginx

Then visit:
http://<VM_PUBLIC_IP>

