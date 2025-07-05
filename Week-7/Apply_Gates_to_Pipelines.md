# Apply Gates to Azure DevOps Pipeline

## Objective
Configure and use **Release Gates** in Azure DevOps to automatically evaluate external conditions before proceeding to the next stage in a release pipeline.

 Reference: [Microsoft Docs – Release Gates](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/approvals/gates?view=azure-devops)

---

## What Are Gates?
Gates are automated checks that evaluate conditions or call external services before a release stage begins. If the gate conditions fail, the release is delayed or stopped.

Common use cases:
- Monitoring Azure Functions or REST APIs
- Checking incident data from ServiceNow
- Querying work items
- Validating deployment status

---

## Example Scenario
You have a release pipeline with two stages:
1. **Stage 1**: Deploy to Test Environment
2. **Stage 2**: Deploy to Production (only if gates pass)

---

## Step-by-Step: Apply Gates to a Pipeline

### Step 1: Navigate to Release Pipelines
1. Go to [https://dev.azure.com](https://dev.azure.com)
2. Select your organization and project.
3. Click **Pipelines > Releases**.
4. Open an existing release pipeline or create a new one.

---

### Step 2: Open Pre-deployment Conditions
1. Click the **Stage** (e.g., Production).
2. Click the **Pre-deployment conditions** icon (person + checkmark).
3. Enable **Gates** toggle.

---

### Step 3: Add Gate Conditions

You can configure one or more of the following:

#### Option 1: Invoke Azure Function
- Select **Invoke Azure Function**
- Set the function URL and authentication method.

#### Option 2: Call REST API
- Choose **Invoke REST API**
- Enter endpoint URL
- Define success criteria (e.g., `"status": "success"`)

#### Option 3: Query Work Items
- Use **Query Work Items** option to check for open bugs before deploying.

---

### Step 4: Configure Evaluation Settings
- **Gate timeout**: Max time gates are evaluated (e.g., 30 minutes)
- **Sampling interval**: Frequency of checking the gate (e.g., every 5 minutes)
- **Minimum success duration**: How long gate must pass continuously (e.g., 10 minutes)

---

### Step 5: Save and Create a Release
- Save the pipeline.
- Create and trigger a new release.
- Observe the **gates evaluation status** before the Production stage executes.

---

## Example: REST API Gate

```json
{
  "status": "success"
}
```

This response from your endpoint will be used to determine if the gate passes. You can host such an endpoint in Azure App Service, AWS Lambda, or a webhook provider.

---

## Best Practices
- Use gates to enforce quality and compliance before production deployments.
- Monitor key metrics or ticketing system status using REST APIs.
- Combine gates with manual approval for high-assurance pipelines.

---

## References
- Release Gates: https://learn.microsoft.com/en-us/azure/devops/pipelines/release/approvals/gates?view=azure-devops