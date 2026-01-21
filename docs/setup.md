# Azure Environment Setup

## Overview
This document outlines the Azure resources created for the Netflix Data Platform project.

## Resource Group
- **Name**: `cm_netflix_project`
- **Purpose**: Container for all project resources
- **Region**: South Africa North

## Azure Data Lake Storage Gen2

### Storage Account
- **Name**: `cmnetflixprojectdl`
- **Type**: Azure Data Lake Storage Gen2 (Hierarchical namespace enabled)
- **Purpose**: Centralized data lake for all pipeline layers

### Container Structure

| Container | Purpose | Data State |
|-----------|---------|------------|
| `raw` | Source data staging | Original CSV files from GitHub |
| `bronze` | Raw ingestion layer | Minimal transformation, full history |
| `silver` | Cleaned data layer | Validated, deduplicated, standardized |
| `gold` | Analytics layer | Business aggregations, ready for consumption |

**Naming Convention**: Follows Medallion Architecture pattern
- **Raw**: Temporary staging for source files
- **Bronze/Silver/Gold**: Delta Lake tables with increasing refinement


## Azure Data Factory

### Data Factory Instance
- **Name**: `cm-adf-netflixproject`
- **Version**: V2
- **Purpose**: Orchestration and data movement
- **Integration Runtime**: Azure AutoResolve (default)

### Planned Components
- **Linked Services**: 
  -  `github_connection` - HTTP connection to GitHub raw data
  -  `datalake_connection` - Azure Data Lake Storage Gen2 connection
  -  Azure Databricks connection (to be created)
  - 
- **Datasets**: 
  - Parameterized CSV datasets for source files
  - Delta Lake datasets for Bronze/Silver/Gold layers
  
- **Pipelines**:
  - Master ingestion pipeline
  - Validation and error handling logic
  - Databricks workflow triggers


## Azure Databricks (To Be Created)

### Workspace
- **Name**: [Will be documented when created]
- **Pricing Tier**: [Standard/Premium - to be selected]
- **Purpose**: Data transformation and Delta Lake processing

### Clusters
- [Details to be added during setup]


## Resource Naming Convention

Following Azure best practices:
- **Prefix**: `cm` (my initials)
- **Resource Type**: `adf`, `dl` (data lake), `dbw` (Databricks)
- **Project**: `netflixproject`
- **Pattern**: `{prefix}-{resource-type}-{project-name}`


## Next Steps

- [ ] Create Databricks workspace
- [ ] Configure ADF linked services
- [ ] Set up service principal for authentication (if needed)
- [ ] Configure RBAC permissions
- [ ] Create ADF pipelines
- [ ] Deploy Databricks notebooks


## Access & Security

- **Authentication**: [Azure AD / Access Keys]
- **Network**: [Public / Private endpoint]
- **Encryption**: Enabled by default for storage


## Cost Considerations

- Storage: Pay-as-you-go based on data volume
- ADF: Charged per activity run and data movement
- Databricks: Will be charged based on DBU consumption


*Last Updated: [20/01/2026]*
