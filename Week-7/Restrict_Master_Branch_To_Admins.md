# Azure DevOps Branch Policy: Restrict Master Access to Project Administrators

## Objective
To restrict access to the `master` (or `main`) branch in Azure DevOps such that **only Project Administrators** can push or modify it, and **Contributors** cannot.

---

##  Step-by-Step Instructions

###  Prerequisite
- You must have **Project Administrator** permissions.

---

### Step 1: Open Your Project
1. Go to [Azure DevOps](https://dev.azure.com).
2. Navigate to your **organization** and open your **project**.

---

### Step 2: Go to Repos > Branches
1. Click **Repos** in the left sidebar.
2. Click **Branches** under Repos.
3. Locate the `master` (or `main`) branch.

---

### Step 3: Open Branch Security Settings
1. Click the **3-dot menu (⋮)** beside `master`.
2. Select **Branch security**.

---

### Step 4: Deny Permissions to Contributors
1. In the **Security window**, select the **Contributors** group.
2. Set the following permissions to **Deny**:
   - **Contribute**
   - **Force push (rewrite history, delete branches)**
   - **Create branch**
3. Click **Save changes**.

---

### Step 5: Allow Permissions to Project Administrators
1. Select the **Project Administrators** group.
2. Set the following permissions to **Allow**:
   - **Contribute**
   - **Force push**
   - **Create branch**
3. Click **Save changes**.

---

###  Optional: Apply Branch Policies for Additional Security
1. Go to **Repos > Branches** again.
2. Click the **3-dot menu (⋮)** next to `master` > **Branch policies**.
3. Configure policies like:
   - Require pull request reviewers.
   - Require successful builds.
   - Limit merge types (e.g., squash, no fast-forward).

---

##  Validation
- Attempt a push as a **Contributor**: Should be blocked.
- Attempt a push as a **Project Administrator**:  Should be allowed.

---

*This policy secures the master branch from unauthorized contributions and enforces role-based access control.*
