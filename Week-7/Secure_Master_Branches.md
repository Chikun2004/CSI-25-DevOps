# Secure Master Branch: Contributors Can Create PRs but Not Merge Directly

## Objective
Configure branch security in Azure DevOps to allow **Contributors to create Pull Requests** but **prevent direct merges or pushes to the `master` branch**.

---

## Prerequisites
- Azure DevOps project with Git repositories
- Must be a Project Administrator or have permission to modify branch security

---

## Step 1: Open Azure DevOps Project
1. Go to [https://dev.azure.com](https://dev.azure.com).
2. Select your organization and project.

---

## Step 2: Navigate to Repos > Branches
1. Click on **Repos** in the left menu.
2. Select **Branches**.
3. Locate the `master` (or `main`) branch.
4. Click the **three-dot menu (⋮)** next to `master` and select **Branch security**.

---

## Step 3: Configure Security for Contributors

### Select Contributor Group
1. In the **Security** dialog, select the **Contributors** group from the list.

### Set Permissions
Modify the following permissions:

| Permission                              | Value       |
|-----------------------------------------|-------------|
| **Contribute**                          | Deny        |
| **Force push (rewrite history)**        | Deny        |
| **Create branch**                       | Allow       |
| **Read**                                | Allow       |
| **Create tag**                          | Allow       |
| **Contribute to pull requests**         | Allow       |

> This ensures Contributors can submit Pull Requests but **not push or merge directly** to `master`.

---

## Step 4: Configure Project Administrators (Optional)
1. In the same **Branch security** screen, select the **Project Administrators** group.
2. Ensure they have the following:

| Permission                  | Value  |
|-----------------------------|--------|
| **Contribute**              | Allow  |
| **Force push**              | Allow  |
| **Create branch**           | Allow  |
| **Manage permissions**      | Allow  |
| **Contribute to pull requests** | Allow |

---

## Step 5: Test the Setup

### As a Contributor:
- Try pushing code directly to `master`:  should be denied.
- Create a Pull Request to `master`:  allowed.

### As an Admin:
- Can complete the PR or merge changes:  allowed.

---

## Best Practices
- Combine this setup with **branch policies** (e.g., required reviewers, build validation).
- Use **protected branches** for `main`, `release`, and `production`.
- Regularly audit permissions using the **Security** tab.

---

## References
- Azure DevOps Branch Permissions:  
  https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-permissions?view=azure-devops