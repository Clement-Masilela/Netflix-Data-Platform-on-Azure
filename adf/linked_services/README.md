# Linked Services

## Overview
Linked services in Azure Data Factory define the connection information needed to connect to external resources. They are similar to connection strings.
I am using these to connect to the raw data in github and connection to datalake

## Active Linked Services

### 1. github_connection
- **Type**: HTTP
- **Purpose**: Connect to GitHub repository to fetch raw CSV files
- **Base URL**: `https://raw.githubusercontent.com/`
- **Authentication**: Anonymous (public GitHub repository)
- **Usage**: Source connection for ingestion pipelines

**Datasets Using This Connection**:
- Parameterized CSV datasets for all Netflix data files
- Dynamic file path based on pipeline parameters

### 2. datalake_connection
- **Type**: Azure Data Lake Storage Gen2
- **Purpose**: Connect to project data lake for Bronze, Silver, Gold layers
- **Storage Account**: `cmnetflixprojectdl`
- **Authentication Method**: [Account Key / Managed Identity / Service Principal]
- **Containers**: 
  - `raw` - Staging area
  - `bronze` - Raw Delta tables
  - `silver` - Cleaned Delta tables  
  - `gold` - Analytics-ready tables

**Datasets Using This Connection**:
- Delta Lake datasets for all medallion layers
- Parameterized paths for dynamic table access


## Connection Architecture
```
GitHub Repository (Source)
        ↓
  github_connection
        ↓
    ADF Pipeline
        ↓
  datalake_connection
        ↓
ADLS Gen2 (bronze/silver/gold)
```


## Security Notes
- GitHub connection uses anonymous access (public repo)
- Data Lake connection should use Managed Identity in production
- Credentials stored in Azure Key Vault (best practice for production)


## Testing Connections
Both connections should be tested in ADF. Both connections were tested and successful in ADF. 

*Linked services are reusable across multiple datasets and pipelines*
