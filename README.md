
# Fabric Capacity Metrics & Cost Chargeback Notebook

## Overview
This repository contains a Python notebook that aggregates Microsoft Fabric capacity usage metrics and cost data to calculate chargeback amounts per workspace using CU% consumption. It extracts:
- Capacity metrics from the Microsoft Fabric Capacity Metrics app
- Fabric capacity cost from Azure Cost Management APIs
- Workspace-level chargeback allocation stored into a Fabric Lakehouse

## Prerequisites
### 1. Service Principal
Create a service principal (App Registration) in Microsoft Entra ID and capture the Tenant ID, Client ID, and Client Secret.
Documentation: https://learn.microsoft.com/en-us/entra/identity-platform/howto-create-service-principal-portal

### 2. Fabric Workspace
Create a Fabric workspace where this notebook will run. Add the service principal as **Contributor**.
Documentation: https://learn.microsoft.com/en-us/fabric/fundamentals/create-workspaces

### 3. Cost Management Reader Role
Assign the **Cost Management Reader** role to the service principal at the subscription scope.
Documentation: https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/assign-access-acm-data

### 4. Azure Key Vault
Store the following secrets:
- Tenant ID
- Service Principal Client ID
- Service Principal Secret
Documentation: https://learn.microsoft.com/en-us/azure/key-vault/secrets/quick-create-portal

### 5. Managed Private Endpoint
Create a Managed Private Endpoint in Fabric to securely access Key Vault.
Documentation: https://learn.microsoft.com/en-us/fabric/security/security-managed-private-endpoints-create

### 6. Capacity Metrics App
Install the Microsoft Fabric Capacity Metrics app.
Documentation: https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app-install

Add the user, who is running the notebook, as **Contributor** to the metrics app workspace.
Assign this workspace to an **F capacity**.

### 7. Lakehouse
Create a Lakehouse and attach it to the notebook.
Documentation: https://learn.microsoft.com/en-us/fabric/onelake/create-lakehouse-onelake

No schema creation is required—this notebook creates all needed tables.

## Setup Instructions
1. Configure notebook variables (capacity_metrics_ws_name & capacity_metrics_target_dataset_name).
2. Ensure the Lakehouse is attached.
3. Run the notebook (Workspace Level Chargeback Query.ipynb) to:
   - Retrieve capacity metrics
   - Retrieve Fabric cost data
   - Calculate per-workspace cost allocation
   - Write output tables to the Lakehouse
4. Validate generated Delta tables inside the Lakehouse.

## Automation
### Option 1: Job Scheduler
Use the built-in Fabric Scheduler to automate notebook execution.
Documentation: https://learn.microsoft.com/en-us/fabric/fundamentals/job-scheduler

### Option 2: Data Factory Pipelines
Use a Data Factory pipeline with a Notebook activity and schedule trigger.
Documentation: https://learn.microsoft.com/en-us/fabric/data-factory/pipeline-runs

## Additional Documentation
- Fabric Capacity Metrics app: https://learn.microsoft.com/en-us/fabric/enterprise/metrics-app
- SemPy Fabric REST Client: https://learn.microsoft.com/en-us/python/api/semantic-link-sempy/sempy.fabric.fabricrestclient
- Key Vault Secure Access: https://learn.microsoft.com/en-us/azure/key-vault/general/security-overview

