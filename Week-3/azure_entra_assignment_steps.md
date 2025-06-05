# Azure Assignment: Subscriptions, Microsoft Entra ID, RBAC & Custom Role  
  

---

## 1  Sign in to Azure Portal  

1. Browse to <https://portal.azure.com>.  
2. Sign in with your personal Microsoft account.  
3. Confirm you are in the **correct directory** (top‑right corner).  

---

## 2  Observe Assigned Subscriptions  

1. From the left‑hand nav, choose **Subscriptions**.  
2. Note the *Subscription ID*, *Display Name*, and *Billing scope*.  
3. Click **My Role** ➜ confirm you are *Owner* or *Contributor*.  


---

## 3  Observe or Create a Microsoft Entra ID Tenant  

### 3.1 Observe Existing Tenant  

1. Select **Microsoft Entra ID** (formerly Azure AD).  
2. In **Overview**, record the *Tenant name* and *Tenant ID*.  

### 3.2 Create a New Tenant (optional)  

> Skip if one tenant is sufficient for the assignment.  

1. **Microsoft Entra ID** ➜ **Manage tenants** ➜ **+ Create**.  
2. Choose **Microsoft Entra ID** › **Create a new tenant**.  
3. Fill in **Organization name**, **Initial domain**, and **Country/Region**.  
4. Click **Review + create** ➜ **Create**.  
5. Switch to the new tenant (notification banner).  
  

---

## 4  Create Test Users  

1. Still inside **Microsoft Entra ID**, select **Users** ➜ **+ New user**.  
2. Choose **Create user**.  
3. Fill in:  
   * **User principal name** – `testuser1@\<yourdomain\>.onmicrosoft.com`  
   * **Name** – Test User 1  
   * **Password** – Auto‑generate (copy it).  
4. Click **Create**.  
5. Repeat for `testuser2`.  


---

## 5  Create Test Group  

1. **Groups** ➜ **+ New group**.  
2. **Group type** = Security, **Group name** = `Support‑Team`, **Membership type** = Assigned.  
3. **Create**.  
4. Open the group ➜ **Members** ➜ **+ Add members** ➜ add *Test User 1 & 2*.  


---

## 6  Assign a Built‑in RBAC Role  

> **Goal:** Give *Test User 1* **Reader** access to the subscription.  

1. Return to **Subscriptions** ➜ select your subscription.  
2. **Access control (IAM)** ➜ **+ Add** ➜ **Add role assignment**.  
3. **Role** = Reader ➜ **Next**.  
4. **Assign access to** = User, **Members** = Test User 1 ➜ **Next** ➜ **Review + create** ➜ **Create**.  

### Verify  

*In an InPrivate window:*  

```text
https://portal.azure.com
```

1. Sign in as `testuser1@…` using the temporary password.  
2. Attempt to **Create a resource** ➜ Should be denied.  
3. Browse **Resource groups** ➜ Should list resources (read‑only).  


---

## 7  Create a Custom RBAC Role  

> **Scenario:** Allow listing **Support tickets** but no other actions.  

1. Staying in the subscription, **Access control (IAM)** ➜ **Roles** tab.  
2. **+ Create** ➜ **Add custom role**.  
3. **Basics**  
   * **Name** = `Support Ticket Reader`  
   * **Description** = Read‑only access to support tickets.  
4. **Permissions** ➜ **+ Add permissions**  
   * Search “`Microsoft.Support/*/read`” → select **`Microsoft.Support/*/read`**.  
   * **Add**.  
5. **Scope** ➜ keep **Subscription** scope.  
6. **JSON** & **Review + create** ➜ **Create**.  

---

## 8  Assign the Custom Role to a Group  

1. Subscription ➜ **Access control (IAM)** ➜ **+ Add** ➜ **Add role assignment**.  
2. **Role** = Support Ticket Reader ➜ **Next**.  
3. **Assign access to** = Group, **Members** = Support‑Team ➜ **Next** ➜ **Review + create** ➜ **Create**.  

### Verify  

1. Sign in as *Test User 2*.  
2. Open **Help + support** ➜ **New support request** should be **disabled**.  
3. **All support requests** should list tickets (read‑only).  


---

## 9  Clean‑Up (Optional)  

* Delete custom role: **Roles** ➜ select role ➜ **Delete**.  
* Remove role assignments on **Access control (IAM)** ➜ **Role assignments**.  
* Delete test users, group, and extra tenant if created.  

---


## Appendix A — Azure CLI Snippets  

```bash
# Login
az login

# List subscriptions
az account list --output table

# Assign Reader role
az role assignment create   --assignee testuser1@YOURDOMAIN.onmicrosoft.com   --role Reader   --subscription SUBSCRIPTION_ID

# Create custom role (JSON file required)
az role definition create --role-definition support-ticket-reader.json
```  