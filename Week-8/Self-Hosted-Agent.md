# Create a Linux/Windows Self-Hosted Agent in Azure DevOps

## Objective
Set up a self-hosted build agent on a Linux or Windows machine to run CI/CD pipelines using your own infrastructure for better control, customization, and performance.

---

## Prerequisites
- An active Azure DevOps organization and project.
- A personal or enterprise machine (Linux or Windows) with internet access.
- Project Administrator or Agent Pool Administrator permissions in Azure DevOps.

---

## Step 1: Navigate to Agent Pools in Azure DevOps
1. Go to [https://dev.azure.com](https://dev.azure.com).
2. Select your **Organization**.
3. Click on the **Project Settings** (bottom-left corner).
4. Under **Pipelines**, click on **Agent Pools**.
5. Choose an existing pool (like `Default`) or click **Add pool** to create a new one:
   - Provide a **name** (e.g., `SelfHostedPool`).
   - Set visibility (organization-wide or project-scoped).
   - Click **Create**.

---

## Step 2: Download the Agent

### A. For **Linux**
1. Select the pool (e.g., `SelfHostedPool`).
2. Click **New agent**.
3. Choose **Linux** as the operating system.
4. Copy the download command. Example:
   ```bash
   mkdir myagent && cd myagent
   curl -O https://vstsagentpackage.azureedge.net/agent/3.236.1/vsts-agent-linux-x64-3.236.1.tar.gz
   tar zxvf vsts-agent-linux-x64-3.236.1.tar.gz

B. For Windows
Select the pool and click New agent.

Choose Windows as the operating system.

Download the agent .zip file manually or use PowerShell:

mkdir agent ; cd agent
Invoke-WebRequest -Uri https://vstsagentpackage.azureedge.net/agent/3.236.1/vsts-agent-win-x64-3.236.1.zip -OutFile agent.zip
Expand-Archive agent.zip
-Step 3: Configure the Agent
A. Linux Agent Configuration
./config.sh
Enter your Azure DevOps organization URL (e.g., https://dev.azure.com/yourorg).

Choose the agent pool (e.g., SelfHostedPool).

Provide a name for the agent (e.g., linux-agent-01).

Choose authentication type (usually PAT – Personal Access Token).

Enter your PAT (can be generated from User Settings > Personal Access Tokens).

B. Windows Agent Configuration
.\config.cmd
Provide the same details as above (URL, pool name, agent name, PAT).

Step 4: Run the Agent
A. Linux
./run.sh
B. Windows
.\run.cmd
The agent should now connect and show online in the Agent Pools dashboard.

Step 5: Configure Agent as a Service (Optional but Recommended)
A. Linux
sudo ./svc.sh install
sudo ./svc.sh start
B. Windows
.\svc install
.\svc start
Step 6: Use Agent in a Pipeline
Example YAML Pipeline
trigger:
- main

pool:
  name: SelfHostedPool

steps:
- script: echo "Running on a self-hosted agent"
Step 7: Validate the Agent
Commit and push the pipeline YAML to your repo.

Monitor the pipeline execution using Azure DevOps portal.

Verify the job is assigned to your SelfHostedPool agent.

Best Practices
Keep the agent software up to date (config.sh --upgrade).

Use firewall rules and antivirus software to secure the agent machine.

Set up auto-restart on failure or system boot.

Assign resource limits if multiple agents run on the same machine.

Monitor disk space, memory, and CPU usage on the self-hosted machine.