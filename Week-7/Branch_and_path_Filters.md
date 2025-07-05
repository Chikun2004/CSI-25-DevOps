# Azure DevOps: Apply Branch Filters and Path Filters

##  Objective
To apply **branch filters** and **path filters** in Azure DevOps to control how branch policies are triggered and enforced.

 Reference: [Microsoft Docs – Branch Permissions](https://learn.microsoft.com/en-us/azure/devops/repos/git/branch-permissions?view=azure-devops)

---

##  Step-by-Step Guide

###  Step 1: Open Your Azure DevOps Project
1. Go to [https://dev.azure.com](https://dev.azure.com).
2. Select your **organization** and open the relevant **project**.

---

###  Step 2: Navigate to Repos > Branches
1. Click **Repos** from the left menu.
2. Select **Branches**.
3. Click the **3-dot menu (⋮)** next to your target branch (e.g., `master` or `main`).
4. Choose **Branch policies**.

---

##  Step 3: Apply Branch Filters

Branch filters allow you to trigger policies only on specific branches.

1. In the branch policy configuration screen, locate **Build Validation** or **Auto-complete** settings.
2. Under **Trigger**, you will find **Branch filters**.
3. Click **Add branch filter** and specify patterns like:
   - `refs/heads/master`
   - `refs/heads/feature/*`
   - `+refs/heads/release/*` (to include)
   - `-refs/heads/experimental/*` (to exclude)

 Use `+` to include and `-` to exclude branches from policy.

---

##  Step 4: Apply Path Filters

Path filters allow policies to be applied only when files in certain directories are changed.

1. Scroll to **Path filters** in the policy configuration.
2. Click **Add a path filter**.
3. Enter directory or file paths, for example:
   - `/src/` → Only trigger when code in `/src/` is modified.
   - `/docs/` → Trigger when documentation changes.
4. Use `+` to include and `-` to exclude paths.
   - `+src/*` → Apply policy when files in `src/` change.
   - `-tests/*` → Ignore changes in the `tests/` folder.

---

##  Step 5: Save and Validate
- Click **Save** after configuring branch and path filters.
- Test the setup by committing to the filtered branch or path and observing the triggered policies.

---

##  Best Practices
- Use branch filters to target policies for `main`, `release/*`, or `feature/*` branches.
- Use path filters to trigger policies only for business-critical code areas.
- Combine with build validation and reviewers for full pipeline control.

---