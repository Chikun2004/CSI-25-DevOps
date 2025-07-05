# Azure DevOps Project: User Groups & Group Policies

## Objective
To create a project in Azure DevOps, define different user groups, and implement group-specific policies to manage permissions and workflow.

---

## Prerequisites
- Azure DevOps account: [https://dev.azure.com](https://dev.azure.com)
- Admin access to an Azure DevOps Organization

---

## Step 1: Create a New Project in Azure DevOps
1. Go to [Azure DevOps](https://dev.azure.com/).
2. Click on **New Project**.
3. Fill in the following details:
   - **Project Name**: e.g., `DevOpsPolicyProject`
   - **Visibility**: Private (recommended for controlled access)
   - Click **Create**.

---

## Step 2: Define User Groups
Common user groups:
- **Project Administrators**
- **Contributors**
- **Readers**
- **Custom Group** (e.g., `QA Team`)

### Create a Custom Group:
1. Navigate to your project.
2. Go to **Project settings > Permissions**.
3. Click **New Group** > Name it e.g., `QA Team`.
4. Add users to this group.

---

## Step 3: Add Users to Project and Groups
1. Go to **Organization Settings > Users**.
2. Click **Add users**.
3. Enter email addresses.
4. Choose **Access level** (Basic/Stakeholder).
5. Assign them to one or more groups.

📚 Reference: [Add users to a project](https://learn.microsoft.com/en-us/azure/devops/organizations/security/add-users-team-project?view=azure-devops&tabs=preview-page)

---

## Step 4: Set Permissions for Each Group
1. Go to **Project Settings > Permissions**.
2. Select a group (e.g., `Contributors`).
3. Modify permissions as needed:
   - **Edit work items**: Allow
   - **Delete work items**: Deny (optional)
   - **View builds**: Allow
   - etc.

Repeat for each group.

---

## Step 5: Implement Branch Policies
1. Go to **Repos > Branches**.
2. Click the 3 dots next to `main` branch > **Branch policies**.
3. Configure policies:
   - **Require pull request reviewers**
   - **Check for linked work items**
   - **Limit merge types**
   - etc.
4. Save changes.

---

## Step 6: Set Build/Release Permissions
1. Go to **Pipelines > Pipelines** or **Releases**.
2. Click the 3 dots > **Security**.
3. Set appropriate access:
   - **Edit build pipeline**: Allow for Devs
   - **View build pipeline**: Allow for QA
   - etc.

---

## Step 7: Audit and Maintain Policies
1. Periodically check user activity and permissions.
2. Revoke or reassign access as teams change.
3. Document policies and update them regularly.


