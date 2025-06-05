# Azure CLI Guide: Azure Key Vault Setup and Secret Management

##  Objective
Create an Azure Key Vault, store secrets, configure access policies, and retrieve secrets using Azure CLI step-by-step.

---

##  Prerequisites

- Azure CLI installed: [Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- Logged in to Azure:
```bash
az login
```
- Subscription selected:
```bash
az account set --subscription "<your-subscription-name-or-id>"
```

---

##  Step 1: Set Variables

```bash
RESOURCE_GROUP="MyKeyVaultRG"
LOCATION="eastus"
KEY_VAULT_NAME="MyUniqueKeyVault1234"  # Must be globally unique
SECRET_NAME="MySecret"
SECRET_VALUE="P@ssword123"
```

---

##  Step 2: Create a Resource Group

```bash
az group create --name $RESOURCE_GROUP --location $LOCATION
```

---

##  Step 3: Create the Azure Key Vault

```bash
az keyvault create   --name $KEY_VAULT_NAME   --resource-group $RESOURCE_GROUP   --location $LOCATION   --sku standard
```

---

##  Step 4: Store a Secret in the Key Vault

```bash
az keyvault secret set   --vault-name $KEY_VAULT_NAME   --name $SECRET_NAME   --value $SECRET_VALUE
```

---

##  Step 5: Configure Access Policy for Current User

```bash
az keyvault set-policy   --name $KEY_VAULT_NAME   --upn $(az ad signed-in-user show --query userPrincipalName -o tsv)   --secret-permissions get list set delete
```

---

##  Step 6: Retrieve a Secret from the Key Vault

```bash
az keyvault secret show   --vault-name $KEY_VAULT_NAME   --name $SECRET_NAME   --query value   -o tsv
```

---

##  Step 7: (Optional) Configure Access Policy for an App (by client ID)

```bash
az keyvault set-policy   --name $KEY_VAULT_NAME   --spn <app-client-id>   --secret-permissions get list
```

---

##  Optional: Clean-Up Resources

```bash
az group delete --name $RESOURCE_GROUP --yes --no-wait
```

---

##  Summary

| Step | Description |
|------|-------------|
| 1    | Define key variables |
| 2    | Create a resource group |
| 3    | Create a Key Vault |
| 4    | Store a secret |
| 5    | Set access policies |
| 6    | Retrieve a secret |
| 7    | (Optional) Add app access |
| 8    | Clean-up resources |

---

