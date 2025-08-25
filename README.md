# AI Chatbot Terraform Infrastructure

This file contains Terraform code to provision and manage the Azure infrastructure for the AI Chatbot project.

## Overview

The infrastructure includes:
- **Resource Group**: Central container for all resources.
- **Azure Cosmos DB**: NoSQL database with multiple containers, each with configurable partition keys.
- **Azure Storage Account**: For storing files and function app code.
- **Azure Function App**: Serverless compute for chatbot logic.
- **Azure Application Insights**: Monitoring and logging.
- **Azure OpenAI**: Cognitive services for AI capabilities.

## Structure

| File/Folder                        | Description                                               |
|------------------------------------|-----------------------------------------------------------|
| `variables.tf`                     | Defines input variables for all Azure resources           |
| `*.tfvars`                         | Environment-specific variable configurations              |
| `providers.tf`                     | Azure provider configuration                              |
| `monitoring.tf`                    | Log Analytics workspace setup                             |
| `modules/`                         | Modular resource definitions for storage, cosmos db, openai, function app, and app insights |
| `main.tf`                          | Root module orchestrating all infrastructure components   |
| `backend.tf`                       | Terraform state backend configuration                     |
| `.github/workflows/chatbot-ci.yml` | CI/CD pipeline for multi-environment deployment           |

- `chatbot/variables.tf`: Defines all input variables for the infrastructure.
- `chatbot/main.tf`: Main resources and modules for provisioning.
- `chatbot/modules/`: Contains reusable modules (e.g., `cosmos_db`).
- `terraform.tfvars` or `prod.tfvars`: Environment-specific values (container names, partition keys, etc.).

## How to Use

1. **Install Terraform**  
   Download and install Terraform from [terraform.io](https://www.terraform.io/downloads.html).

2. **Configure Azure Credentials**  
   Authenticate with Azure CLI:
   ```bash
   az login
   ```

<!-- 3. **Set Variable Values**  
   Edit `terraform.tfvars` to provide values for your environment:
   ```hcl
   resource_group_name = "ai-chatbot"
   location            = "East US"
   subscription_id     = "your-subscription-id"

   cosmos_db = {
     cosmos_db_account_name = "your-cosmos-account"
     database_name          = "your-db"
     containers = {
       container1 = { partition_key_path = "/rawdataid" }
       container2 = { partition_key_path = "/vectorembeddingsid" }
       container3 = { partition_key_path = "/metadataid" }
       container4 = { partition_key_path = "/sharepointrawdataid" }
     }
   } -->

   # Add other variables as needed
   ```

4. **Initialize Terraform**
   ```sh
   terraform init
   ```

5. **Plan the Deployment**
   ```sh
   terraform plan
   ```

6. **Apply the Deployment**
   ```sh
   terraform apply
   ```

## Best Practices

- Use separate state files or workspaces for each environment (dev, qa, prod).
- Store sensitive values (like secrets) in a secure backend or use environment variables.
- Use `for_each` and maps to avoid code repetition for similar resources (e.g., Cosmos DB containers).

## Clean Up

To destroy all resources created by this configuration:
```sh
terraform destroy
```

## Notes

- Make sure you have the necessary permissions in your Azure subscription.
- Review and update the `terraform.tfvars` file for your specific environment and naming conventions.


