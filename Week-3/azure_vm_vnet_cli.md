# Azure CLI Guide: Create Virtual Machine and Virtual Network (VNet)

##  Objective
Create a Virtual Network (VNet) and a Virtual Machine (VM) in Azure using the Azure CLI step-by-step.

---

##  Prerequisites

- Azure CLI installed ([Install Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli))
- Logged in to Azure via CLI:
```bash
az login
```

---

##  Step 1: Set Default Variables

```bash
# Set your variables
RESOURCE_GROUP="MyResourceGroup"
LOCATION="eastus"
VNET_NAME="MyVNet"
SUBNET_NAME="MySubnet"
NSG_NAME="MyNSG"
VM_NAME="MyVM"
IP_NAME="MyPublicIP"
NIC_NAME="MyNIC"
IMAGE="UbuntuLTS"
USERNAME="azureuser"
```

---

##  Step 2: Create a Resource Group

```bash
az group create --name $RESOURCE_GROUP --location $LOCATION
```

---

##  Step 3: Create a Virtual Network and Subnet

```bash
az network vnet create   --resource-group $RESOURCE_GROUP   --name $VNET_NAME   --subnet-name $SUBNET_NAME
```

---

##  Step 4: Create a Network Security Group (Optional)

```bash
az network nsg create   --resource-group $RESOURCE_GROUP   --name $NSG_NAME
```

---

##  Step 5: Create a Public IP Address

```bash
az network public-ip create   --resource-group $RESOURCE_GROUP   --name $IP_NAME
```

---

##  Step 6: Create a Network Interface Card (NIC)

```bash
az network nic create   --resource-group $RESOURCE_GROUP   --name $NIC_NAME   --vnet-name $VNET_NAME   --subnet $SUBNET_NAME   --network-security-group $NSG_NAME   --public-ip-address $IP_NAME
```

---

##  Step 7: Create a Virtual Machine

```bash
az vm create   --resource-group $RESOURCE_GROUP   --name $VM_NAME   --nics $NIC_NAME   --image $IMAGE   --admin-username $USERNAME   --generate-ssh-keys
```

---

##  Step 8: Open Port 22 for SSH (Optional)

```bash
az vm open-port   --port 22   --resource-group $RESOURCE_GROUP   --name $VM_NAME
```

---

##  Step 9: Connect to the VM

```bash
az vm list-ip-addresses   --resource-group $RESOURCE_GROUP   --name $VM_NAME   --output table
```

Use the public IP to connect:
```bash
ssh azureuser@<public-ip>
```

---

##  Optional: Delete All Resources

```bash
az group delete --name $RESOURCE_GROUP --yes --no-wait
```

---

