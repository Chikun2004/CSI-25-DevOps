# Apply Pre and Post-Deployment Approvers in Azure DevOps Release Pipeline

## Objective
Configure **pre-deployment** and **post-deployment** approval checks in Azure DevOps release pipelines to control and audit deployment processes, ensuring that the right people review and approve deployments.

---

## Prerequisites
- Azure DevOps project and release pipeline created.
- You must have **Edit release pipeline** permissions.
- Approvers must be members of the Azure DevOps project or organization.
- Environment(s) already defined in the release pipeline.

---

## Step 1: Open Your Azure DevOps Project
1. Go to [https://dev.azure.com](https://dev.azure.com).
2. Navigate to your **Project**.

---

## Step 2: Navigate to Release Pipelines
1. Go to **Pipelines > Releases** in the left-hand navigation.
2. Open the release pipeline you want to edit (or create a new one if needed).

---

## Step 3: Open Environment Settings
1. In the pipeline visual designer, locate the **Environment** (e.g., Dev, QA, Prod).
2. Hover over the environment box and click the **"Lightning bolt" ⚡ icon** for **Pre-deployment conditions**.
3. Similarly, click the **post-deployment ⚡ icon** for **Post-deployment conditions**.

---

## Step 4: Configure Pre-deployment Approvers
1. Toggle **Pre-deployment approvals** to **Enabled**.
2. Click **Add** under **Approvers**.
3. Select users or groups (e.g., `release-approvers` group, or specific user).
4. Configure other optional settings:
   - **Timeout** (e.g., 2 days)
   - **Re-evaluation options** (if new commits invalidate approval)
   - **Approval policies** (e.g., require all or one)
5. Click **Save**.

---

## Step 5: Configure Post-deployment Approvers
1. Click the **Post-deployment ⚡ icon** on the environment.
2. Toggle **Post-deployment approvals** to **Enabled**.
3. Click **Add** and select approvers as done in the pre-deployment step.
4. Adjust timeout and evaluation settings as needed.
5. Click **Save**.

---

## Step 6: Save and Create a Release
1. Click **Save** in the release pipeline editor.
2. Click **Create Release**.
3. Choose the build artifact and click **Create**.
4. Navigate to the release execution page.

---

## Step 7: Validate Pre/Post-Deployment Approval Flow

### A. Pre-Deployment
1. The release will pause before deploying to the environment.
2. Approvers receive a notification via email or Azure DevOps UI.
3. Approvers must **Approve** or **Reject**.
4. Once approved, the deployment proceeds.

### B. Post-Deployment
1. After deployment completes, Azure DevOps pauses again.
2. Post-deployment approvers are notified.
3. They confirm the deployment was successful and can mark it as **Approved**.

---

## Example Use Cases
- Pre-Deployment: Manager or QA team approves before deploying to Production.
- Post-Deployment: Stakeholder or monitoring team confirms that deployment was successful.

---

## Best Practices
- Use Azure DevOps **security groups** (like `DevOps-Release-Approvers`) for scalable approvals.
- Limit approval permissions to avoid accidental deployments.
- Enable timeout periods to prevent long-running blocked releases.
- Track approval history in **Auditing** for compliance and traceability.

---

## References
- [Configure Pre/Post Deployment Approvals](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/approvals/?view=azure-devops&tabs=yaml)
- [Release Pipelines Overview](https://learn.microsoft.com/en-us/azure/devops/pipelines/release/)
- [YouTube Tutorial - Azure DevOps Deployment Approvals](https://www.youtube.com/watch?v=Cu7zx9u1sOE&pp=ygUXYXp1cmUgZGV2b3BzIHRlc3QgcGxhbnM%3D)
