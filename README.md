
# 🌐 Azure Terraform Multi-Environment Infrastructure

This project provisions Azure infrastructure using a **modular and multi-environment Terraform setup**, supporting clean, reusable, and scalable Infrastructure as Code (IaC) practices across environments such as **dev** and **prod**.

### 🔧 Resources Provisioned

- Resource Group  
- Virtual Network  
- Subnet  
- Network Security Group (NSG)  
- Network Interface Card (NIC)  
- Public IP  
- Virtual Machine (Linux)  
- Storage Account  

---

## 📁 Project Structure

```
azure-terraform-multi-env/
├── modules/                             # Reusable modules (shared across environments)
│   ├── resource-group/
│   ├── virtual-network/
│   ├── subnet/
│   ├── nsg/
│   ├── nic/
│   ├── public_ip/
│   ├── virtual-machine/
│   └── storage-account/
│
├── environments/                        # Separate folders for each environment
│   ├── dev/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfvars
│   │   ├── backend.tf
│   │   └── provider.tf
│   │
│   └── prod/
│       ├── main.tf
│       ├── variables.tf
│       ├── terraform.tfvars
│       ├── backend.tf
│       └── provider.tf
│
└── create-azure-tf-backend.sh           # Script to create remote backend storage
└── README.md                            # Project overview and usage instructions
```

---

## 📦 Modules Overview

Each module contains:
- `main.tf` – Resource definitions
- `variables.tf` – Input variable declarations
- `outputs.tf` – Outputs from the module

### Available Modules:
- `resource-group`
- `virtual-network`
- `subnet`
- `nsg`
- `nic`
- `public_ip`
- `virtual-machine`
- `storage-account`

---

## ✅ Prerequisites

- [Terraform](https://developer.hashicorp.com/terraform/downloads)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- Azure subscription with permission to provision resources

---

## 🔐 Authentication

Authenticate using **Azure CLI**:
```bash
az login
```

Or use a **Service Principal**:
```bash
export ARM_CLIENT_ID=""
export ARM_CLIENT_SECRET=""
export ARM_SUBSCRIPTION_ID=""
export ARM_TENANT_ID=""
```

---
### 📌 Make sure you have a remote backend configured.


## ☁️ Setting Up Remote Backend

Remote state is stored in an Azure Storage Account. Use the included script:

```bash
bash setup-backend.sh
```

This script will:
- Create a resource group
- Create a uniquely named storage account
- Create a blob container named `tfstate`

Then create a `backend.config` file per environment:

```hcl
resource_group_name  = "myResourceGroup"
storage_account_name = "uniquestorage1716720847"
container_name       = "tfstate"
key                  = "dev/terraform.tfstate"
```

Initialize Terraform using:
```bash
terraform init -backend-config="backend.config"
```

---

## 📂 Environment Example: `dev`

Navigate to the `dev/` folder:

```bash
cd environments/dev
```

---

## 🔧 Example `terraform.tfvars`

```hcl
location               = "eastus"
resource_group_name    = "rg-dev"

vnet_name              = "vnet-dev"
vnet_address_space     = ["10.0.0.0/16"]

subnet_name            = "subnet-dev"
subnet_address_prefixes = ["10.0.1.0/24"]

nsg_name               = "nsg-dev"
public_ip_name         = "pip-dev"
allocation_method      = "Dynamic"

nic_name               = "nic-dev"
vm_name                = "vm-dev"
vm_size                = "Standard_B1ls"
computer_name          = "devhost"
admin_username         = "azureuser"
public_key_path        = "~/.ssh/id_rsa.pub"
```

⚠️ **Do not commit this file if it contains sensitive values.**

---

## 🚀 Usage Steps

1. **Login to Azure**:
    ```bash
    az login
    ```

2. **Go to an environment** (e.g., `dev`):
    ```bash
    cd environments/dev
    ```

3. **Initialize Terraform**:
    ```bash
    terraform init -backend-config="backend.config"
    ```

4. **Validate your configuration**:
    ```bash
    terraform validate
    ```

5. **Plan the infrastructure**:
    ```bash
    terraform plan -var-file="terraform.tfvars"
    ```

6. **Apply the infrastructure**:
    ```bash
    terraform apply -var-file="terraform.tfvars"
    ```

7. **Output the results**:
    ```bash
    terraform output
    ```

8. **To destroy the environment**:
    ```bash
    terraform destroy -var-file="terraform.tfvars"
    ```

---

## 🔒 Recommended `.gitignore`

```gitignore
*.tfstate
*.tfstate.*
.terraform/
terraform.tfvars
backend.config
*.pem
*.key
*.log
```

---

## 📄 License

This project is open-sourced under the [MIT License](LICENSE).

---

## 🤝 Contributions

Feel free to fork this repo, open issues, and submit pull requests to improve and expand the setup!
