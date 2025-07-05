# Apply Branch Policies and Branch Security in Azure DevOps

## Objective
Configure branch policies and security settings to protect key branches in Azure DevOps, ensuring code quality and preventing unauthorized changes.

## Prerequisites
- You must be a Project Administrator or have **Edit policies** and **Manage permissions** rights.
- A Git repository must be initialized in your Azure DevOps project.

---

## Step 1: Open Azure DevOps Project
1. Navigate to https://dev.azure.com.
2. Select your organization and the target project.

---

## Step 2: Navigate to Branches
1. Go to **Repos > Branches** from the left-hand menu.
2. Identify the branch to protect (e.g., `main` or `master`).

---

## Part A: Apply Branch Policies

### Step A1: Open Branch Policies
1. Click the **three-dot menu (⋮)** next to the target branch.
2. Select **Branch policies**.

### Step A2: Configure the Following Policies
- **Minimum number of reviewers**: Require at least one reviewer before completing a PR.
- **Check for linked work items**: Require linking to work items.
- **Check for comment resolution**: Ensure all comments are addressed.
- **Limit merge types**: Choose allowed merge types (e.g., squash, rebase).
- **Build validation**:
  - Add a build pipeline that must succeed before merging.
- **Automatically include reviewers**: Automatically assign teams/users as reviewers.

### Step A3: Save Policy
Click **Save changes** after configuring.

---

## Part B: Apply Branch Security

### Step B1: Open Branch Security Settings
1. In the **Branches** view, click the **three-dot menu (⋮)** for the branch.
2. Choose **Branch security**.

### Step B2: Configure Permissions per Group/User
You will see permissions for various groups like:
- **Project Administrators**
- **Contributors**
- **Readers**

### Step B3: Common Permission Settings

| Permission                         | Admins     | Contributors | Readers |
|-----------------------------------|------------|--------------|---------|
| Contribute                        | Allow      | Deny         | Deny    |
| Force push (rewrite history)     | Allow      | Deny         | Deny    |
| Create branch                     | Allow      | Allow        | Deny    |
| Delete                            | Allow      | Deny         | Deny    |
| Manage permissions                | Allow      | Deny         | Deny    |

1. Select the group (e.g., Contributors).
2. Set `Contribute`, `Force push`, and other sensitive actions to **Deny**.
3. Set appropriate **Allow** for Project Administrators.
4. Click **Save changes**.

---

## Step C: Validate Security and Policies
- Attempt a direct push to the protected branch from a Contributor account: should be blocked.
- Create a pull request to merge into the branch: should enforce the configured policies.

---

## Best Practices
- Apply branch policies to all production branches (`main`, `release/*`).
- Deny force-push for all non-admin roles.
- Use build validation for automated quality checks.
- Regularly audit branch security settings.

---

## References
- Branch Policies: https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-policies
- Branch Permissions: https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-permissions?view=azure-devops