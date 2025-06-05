# Azure Assignment: Microsoft Entra ID, Subscriptions, and RBAC Role Management

##  Objective
Perform the following operations step-by-step in your Azure account and test role-based access using test users and groups.

---

##  Step 1: Observe Assigned Subscriptions

1. Log in to [Azure Portal](https://portal.azure.com).
2. Navigate to **Subscriptions** from the left sidebar.
3. View your current subscriptions.
4. Note down the **Subscription Name**, **Subscription ID**, and your **Role**.

 *Ensure you are assigned at least the “Owner” or “Contributor” role.*

---

##  Step 2: Observe or Create Azure Entra ID (formerly Azure AD)

### Option A: Observe Existing Azure Entra ID
1. Go to **Microsoft Entra ID**.
2. In the **Overview** section, record the **Tenant Name** and **Tenant ID**.

### Option B: Create New Azure Entra ID
1. Navigate to **Microsoft Entra ID** > **Manage tenants** > **+ Create**.
2. Select **Microsoft Entra ID** > **Create a new tenant**.
3. Fill in:
   - Organization name
   - Initial domain name
   - Country/Region
4. Click **Create** and switch to the new tenant.

---

##  Step 3: Create Test Users and Groups

### 👤 Create Test Users
1. Go to **Microsoft Entra ID** > **Users** > **+ New user**.
2. Enter details:
   - User name: `testuser1`
   - Name: `Test User 1`
   - Let Azure auto-generate a password
3. Click **Create** and repeat for `testuser2`.

### 👥 Create Test Group
1. Go to **Groups** > **+ New group**.
2. Set:
   - Group type: **Security**
   - Group name: `DevOpsTeam`
   - Membership type: **Assigned**
3. Add `testuser1` and `testuser2` as members.
4. Click **Create**.

---

##  Step 4: Assign a Built-in RBAC Role to a User

1. Navigate to **Subscriptions** > Select your subscription.
2. Go to **Access control (IAM)** > **+ Add** > **Add role assignment**.
3. Choose a role, e.g., **Reader**.
4. Assign to `testuser1` and complete the wizard.

###  Test the Role
1. Open a private browser window and log in as `testuser1`.
2. Verify they can only view resources but cannot create or delete them.

---

##  Step 5: Create a Custom RBAC Role and Assign to Group

###  Create Custom Role
1. Go to **Subscriptions** > **Access control (IAM)** > **Roles** > **+ Add custom role**.
2. Enter:
   - Name: `CustomReadSupportRole`
   - Description: Read access to support tickets only
3. Add permission: `Microsoft.Support/*/read`
4. Assign scope to the subscription and click **Create**.

###  Assign Role to Group
1. Go to **Access control (IAM)** > **+ Add** > **Add role assignment**.
2. Choose your custom role.
3. Assign to group `DevOpsTeam`.

###  Test the Role
1. Sign in as `testuser2` in incognito mode.
2. Confirm the user can only read support tickets and nothing else.

---

##  Step 6: Clean-Up (Optional)

- Remove role assignments.
- Delete test users and groups.
- Delete the custom role.
- Optionally, delete the extra tenant.

---

