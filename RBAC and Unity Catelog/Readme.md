# Azure Data Governance

## Azure (UC + RBAC)

- P2 Licensed subscription is needed for RBAC group creation.
- UC and RBAC are accessed through AAD.
- RBAC originated from Azure, while UC originated from DB.

### Perview Lineage vs UC Lineage

| Perview | UC |
| --- | --- |
| Global | Limited to Azure environment |
| Azure service | Used with AWS/Azure/GCP |
| Connects with on-prem | Limited to Azure environment only |
| Scans files on-prem and in the rest of the Azure environment | Only scans files in Azure environment |

- **Perview Scans**: Files on-prem and in the Azure environment. It can collect data from any point in the pipeline (on-prem or cloud) and display it in a dashboard, beneficial for understanding the cost of services used.

### RBAC Group Creation in Microsoft Entra ID

- Creating AAD groups and adding users for access is not a job of the DE.
- **SCIM Connector**: An in-built ADB application used to sync AAD groups from Azure Entra ID to the Databricks account portal.

### Flow: Reflection of AAD Groups in DB Workspace
1. Creation of AAD environment (with P2 subscription)
2. Creation of AAD groups
3. Adding users to AAD
4. Creation of SCIM Connector (Connect it with ADB using tokens and tenant ID from ADB)
5. Add AAD groups in SCIM connectors (After 40 minutes, groups will reflect in the ADB account group level)
6. Verify that these AAD groups are synced with the Databricks account.
7. Add groups from ADB account group to ADB workspace.

- **Account Admins vs Metastore Admins vs Workspace Admins**
  - **DBX Account Admin**: Complete access to all workspaces across environments.
  - **Workspace Admin**: Full access to a specific workspace (Dev/UT/Prod).
  - **Metastore Admin**: Full access to the metastore for a specific ADB workspace.

## Unity Catalog (UC) and Data Governance

### Role Descriptions:
- **Global Admin**: Complete access to the Azure account.
- **Metastore Admin**: Full access to the Databricks account portal and metastore.
- **Workspace Admin**: Full access to a particular Databricks workspace.
- **Unity Catalog Owner**: Full privileges on a catalog and assigns roles to AAD groups.
- **Schema Owner**: Full privileges on a schema and assigns roles to AAD groups.
- **Table Owner**: Full privileges on a table and assigns roles to AAD groups.
- **Storage Credentials Owner**: Full privileges on storage credentials and assigns roles.
- **External Location Owner**: Full privileges on external locations and assigns roles.

### Metastore
- A metastore is the top-level container of objects in Unity Catalog. It registers metadata about data assets and the permissions governing access.

### Unity Catalog (UC) Structure:
- **Project 1**: Includes schemas for different environments like Dev, Test, UAT, and Prod.
  
  Example (before UC):
  - Dev raw schema
  - Dev intermediate schema
  - Dev curated schema
  
  Example (after UC):
  - Dev raw schema
  - Test raw schema
  - UAT raw schema
  - Prod raw schema

- To sync from the Hive Metastore to UC, the cluster needs to be UC enabled.

### Steps for Migrating from Hive Metastore to Unity Catalog:
1. Creation of AAD environment (with P2 Subscription)
2. Creation of AAD groups
3. Adding users to AAD
4. SCIM Connector creation
5. Add AAD groups in SCIM connectors
6. Verify AAD groups sync with the Databricks account
7. Add groups from ADB account to ADB workspace
8. Create managed schemas under Unity Catalog
9. Create storage credentials in Databricks workspace
10. Create external locations
11. Sync schemas from Hive Metastore to Unity Catalog
12. Assign necessary roles to AAD groups.

### Data Lineage:
- Data lineage tracks the life cycle of data, helping to determine its origin, quality, and regulatory compliance. Using UC, you can run scripts and see data lineage in the pipeline, displaying info on the latest pipeline run.

## Unity Catalog Real-World Example:
Imagine a multinational corporation using Azure Databricks for predictive maintenance. Teams such as engineering, IT, and compliance work together on a unified platform provided by Unity Catalog. The engineering team accesses datasets, IT manages permissions, and the compliance team tracks data lineage and access logs. This approach enhances data governance and streamlines operations.

---

Unity Catalog is a core component of the Azure Databricks ecosystem, designed to meet data governance and security needs in modern data landscapes.

