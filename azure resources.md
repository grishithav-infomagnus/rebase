# Azure Services for Chatbot Project

This document outlines the key Azure services required for the Chatbot project. All resources are provisioned separately for each environment: **Development (Dev)**, **Quality Assurance (QA)**, and **Production (Prod)**. This ensures isolation, security, and flexibility for each stage of the application lifecycle.
 
 - To see the naming conventions for each environment, check the `<environment>.tfvars` file. 

## Required Azure Services

### Resource Group
   - Organizes all Azure resources related to the Chatbot project
   - Simplifies management, billing, and access control
   - Separate resource group for each environment (Dev, QA, Prod)

### Service Principal Name (SPN)
  - Provides a secure identity for the chatbot application to access Azure resources
  - Allows automated deployment and management without user interaction

### Azure Function App
  - Hosts serverless backend functions to handle chatbot logic and events
  - Includes a Function App with scheduled triggers (e.g., timer-based jobs for maintenance or data sync)
  - Scales automatically based on demand to handle user interactions
  - Minimizes infrastructure management, allowing faster development
  - Separate Function App for each environment

### Storage Account
  - Stores files, logs, and artifacts required by the chatbot and its components
  - Used for Function App code deployment and persistent storage needs
  - Separate Storage Account for each environment

### App Service Plan (linked to Function App)
  - Provides the underlying compute resources for the Function App
  - Allows scaling and performance configuration for serverless workloads
  - Each Function App is associated with its own App Service Plan per environment

### Azure Cosmos DB
  - Stores chatbot conversation data and user information with low latency
  - Provides global distribution to support users across regions
  - **Database with Shared Throughput**:
    - A single Cosmos DB database is provisioned with shared throughput to optimize cost and performance across containers.
    - **Configured with 4 containers:**
      - `container1`: e.g., for raw data
      - `container2`: e.g., for vector embeddings
      - `container3`: e.g., for metadata
      - `container4`: e.g., for SharePoint raw data
  - Separate Cosmos DB instance for each environment

### Application Insights & Azure Monitoring
  - Monitors chatbot application performance and user interactions
  - Helps detect and diagnose issues in real-time
  - Provides analytics to improve chatbot effectiveness and reliability
  - Azure Monitoring is enabled for all compute and database resources
  - Separate monitoring resources for each environment

### Azure OpenAI
  - Powers advanced natural language understanding for the chatbot
  - Enables generation of human-like responses and conversational flows
  - Supports integration of AI capabilities to enhance user experience
  - Two OpenAI deployments for each environment:
    - **Text Embedding Model**: Used for generating vector embeddings from text for semantic search and retrieval.
    - **Chat Completion Model**: Used for generating conversational responses and handling chat interactions.
  - Separate OpenAI resource for each environment

