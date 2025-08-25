
# AI Chatbot Terraform Infrastructure

This document describes the Terraform codebase used to provision and manage Azure infrastructure for the AI Chatbot project.

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
| `modules/`                         | Modular resource definitions for storage, Cosmos DB, OpenAI, Function App, and App Insights |
| `main.tf`                          | Root module orchestrating all infrastructure components   |
| `backend.tf`                       | Terraform state backend configuration                     |
| `.github/workflows/chatbot-ci.yml` | CI/CD pipeline for multi-environment deployment           |

- `chatbot-infrastructure/modules/`: Contains reusable modules.
- `chatbot-infrastructure/modules/main.tf`: Orchestrates module components
- `chatbot-infrastructure/modules/variables.tf`: Defines input variables for module resources
- `chatbot-infrastructure/modules/output.tf`: Outputs from the modules


## How to Use

1. **Install Terraform**  
   Download and install Terraform from [terraform.io](https://www.terraform.io/downloads.html).

2. **Configure Azure Credentials**  
   Authenticate with Azure CLI:
   ```bash
   az login
   ```


3. **Set Variable Values**
   Edit the appropriate variable file (`dev.tfvars`, `qa.tfvars`, or `prod.tfvars`) to provide values for your environment:
   ```hcl
   resource_group_name    = "<your-resource-group-name>"
   location               = "<your-azure-region>"
   subscription_id        = "<your-subscription-id>"
   cosmosdb_account_name  = "<your-cosmosdb-account-name>"
   storage_account_name   = "<your-storage-account-name>"
   function_app_name      = "<your-function-app-name>"
   # Add other variables as needed
   ```

   **Why use three different tfvars files (dev.tfvars, qa.tfvars, prod.tfvars)?**
   - **Environment Isolation:** Each environment (development, QA, production) often requires different resource names, sizes, or locations to avoid conflicts and ensure proper isolation.
   - **Safe Testing:** Changes can be tested in dev or QA before being applied to production, reducing the risk of outages or data loss.
   - **Access Control:** Different teams or users may have access to only certain environments, and using separate tfvars files helps enforce this separation.
   - **Cost Management:** Resource sizes and counts can be smaller in dev/QA to save costs, while production can use more robust settings.
   - **Configuration Flexibility:** You can easily switch between environments by specifying the appropriate tfvars file during plan/apply.


4. **Navigate to the Infrastructure Folder**
   Change directory to the `chatbot-infrastructure` folder:
   ```sh
   cd chatbot-infrastructure
   ```

5. **Initialize Terraform**
   Initialize Terraform and configure the backend for remote state storage:
   ```sh
   terraform init \
     -backend-config="resource_group_name=<your-resource-group>" \
     -backend-config="storage_account_name=<your-storage-account>" \
     -backend-config="container_name=<tfstate-container-name>" \
     -backend-config="key=<tfstate-key>"
   ```



6. **Validate the Configuration**
   Run Terraform's built-in validation to check for syntax errors and internal consistency:
   ```sh
   terraform validate
   ```

7. **Plan the Deployment**
   Generate and review an execution plan. Specify the appropriate variable file for your environment (e.g., `dev.tfvars`, `qa.tfvars`, or `prod.tfvars`):
   ```sh
   terraform plan -var-file=<environment>.tfvars
   ```
   Replace `<environment>` with `dev`, `qa`, or `prod` as needed.

8. **Apply the Deployment**
   Apply the changes using the appropriate variable file for your environment (e.g., `dev.tfvars`, `qa.tfvars`, or `prod.tfvars`):
   ```sh
   terraform apply -var-file=<environment>.tfvars
   ```
   Replace `<environment>` with `dev`, `qa`, or `prod` as needed.

## Best Practices

- Use separate state files or workspaces for each environment (dev, qa, prod).
- Store sensitive values (like secrets) in a secure backend or use environment variables.
- Use `for_each` and maps to avoid code repetition for similar resources (e.g., Cosmos DB containers).

## Clean Up

To destroy all resources created by this configuration:
```sh
terraform destroy -var-file=<environment>.tfvars
```
Replace `<environment>` with `dev`, `qa`, or `prod` as needed.

## Notes

- Make sure you have the necessary permissions in your Azure subscription.
- Review and update the `<environment>.tfvars` file for your specific environment and naming conventions.
- Replace `<environment>` with `dev`, `qa`, or `prod` as needed.



