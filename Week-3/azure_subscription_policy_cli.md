# Azure CLI Guide: Create and Assign a Policy at Subscription Level

##  Objective
Create and assign an Azure Policy at the subscription level using Azure CLI to enforce governance rules.

---

##  Prerequisites

- Azure CLI installed: [Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- Logged in to Azure:
```bash
az login
```
- Your subscription ID:
```bash
az account show --query id -o tsv
```

---

##  Step 1: Set Variables

```bash
SUBSCRIPTION_ID=$(az account show --query id -o tsv)
LOCATION="eastus"
POLICY_DISPLAY_NAME="Allow Only Specific Locations"
POLICY_NAME="allow-only-eastus-locations"
```

---

##  Step 2: Create a Policy Definition

```bash
az policy definition create   --name $POLICY_NAME   --display-name "$POLICY_DISPLAY_NAME"   --description "This policy allows resources to be deployed only in East US"   --rules '{
    "if": {
      "not": {
        "field": "location",
        "equals": "eastus"
      }
    },
    "then": {
      "effect": "deny"
    }
  }'   --mode All   --subscription $SUBSCRIPTION_ID
```

---

##  Step 3: Assign the Policy to the Subscription

```bash
az policy assignment create   --name "enforce-eastus-location"   --display-name "Enforce East US Location Policy"   --policy $POLICY_NAME   --scope /subscriptions/$SUBSCRIPTION_ID
```

---

##  Step 4: Test the Policy

Try to deploy a resource in a region other than East US (e.g., West US) to confirm that the policy blocks the action.

```bash
az group create   --name TestRG   --location westus
```

 You should get a `PolicyViolation` error.

---

##  Step 5: Clean-Up

To delete the policy and its assignment:

```bash
az policy assignment delete --name "enforce-eastus-location"
az policy definition delete --name $POLICY_NAME
```

---

##  Summary

| Step | Description |
|------|-------------|
| 1    | Set variables and get subscription ID |
| 2    | Create a policy that restricts location |
| 3    | Assign it at subscription level |
| 4    | Test policy enforcement |
| 5    | Clean up resources |

---

