# Azure DevOps: Applying Branch Security and Locks

##  Objective
To secure a branch (e.g., `master`) in Azure DevOps by applying security rules and locking it to prevent direct changes.

 Reference Video: [YouTube – Azure Boards Tutorial](https://www.youtube.com/watch?v=4ah5Tuj0i4s&pp=ygUMYXp1cmUgYm9hcmRz)

---

##  Step-by-Step Guide

###  Step 1: Navigate to Azure DevOps Project
1. Visit [https://dev.azure.com](https://dev.azure.com).
2. Open your **organization** and **project**.

---

###  Step 2: Go to Repos > Branches
1. Click on **Repos** from the left navigation pane.
2. Select **Branches**.
3. Locate the branch you want to secure (e.g., `master` or `main`).

---

###  Step 3: Apply Branch Security
1. Click the **3-dot menu (⋮)** next to the branch name.
2. Select **Branch security**.
3. Configure group or user permissions:
   - **Deny** `Contribute` for unwanted users/groups.
   - **Allow** `Contribute` for trusted groups (e.g., Project Administrators).
4. Click **Save changes** after updating each group's permission.

---

###  Step 4: Lock the Branch
1. In the same **3-dot menu (⋮)** for the branch, choose **Lock**.
2. Confirm by clicking **Yes, lock branch**.

 What locking does:
- Prevents all changes directly to the branch (including pushes).
- Users must create a new branch and create a pull request.

---

###  Step 5: Test the Lock
- Try pushing code to the locked branch:
  -  Admins: Should be blocked unless explicitly allowed.
  -  Contributors: Will see a rejection message.

---

###  Step 6: Unlocking (if needed)
1. Go back to **Branches**.
2. Click **3-dot menu (⋮)** next to the locked branch.
3. Select **Unlock**.

---

##  Best Practices
- Use locking during sensitive phases like release or production hotfixes.
- Combine locking with **branch policies** for maximum security.
- Regularly audit branch permissions.

---
