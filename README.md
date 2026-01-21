# Netflix-Data-Platform-on-Azure
Production-grade Azure Data Engineering solution featuring dynamic ADF pipelines, Databricks Autoloader and Delta Lake with Medallion architecture

## Overview 
This project demonstrates a production-grade, end-to-end data engineering solution on Azure, designed for scalability, reusability and modern best practices. It covers ingestion, transformation, and modeling using Azure Data Factory, Databricks, and Delta Lake with Medallion Architecture.

*Key Focus Areas:*
- Scalability and reusability
- Robust error handling and data validation
- Incremental processing with fault tolerance
- Modular, parameterized pipeline design



## Architecture
```
Data Sources → ADF Ingestion → Bronze Layer (Raw) → Silver Layer (Cleaned) → Gold Layer (Business)
                    ↓                ↓                      ↓                       ↓
              Validation         Delta Lake          Transformations          Analytics Ready
```

**Components:**
- **Azure Data Factory (ADF)**: Dynamic, parameterized ingestion pipelines
- **Azure Databricks**: Scalable data processing and transformation
- **Delta Lake**: ACID transactions, schema enforcement, time travel
- **Databricks Autoloader**: Incremental file ingestion
- **Delta Live Tables**: Data quality and lineage management



## Key Features

### 1. **Dynamic Ingestion Pipelines**
- Parameterized ADF pipelines for reusability across multiple data sources
- Built-in data validation logic
- Comprehensive error-handling and retry mechanisms
- Metadata-driven pipeline configuration

### 2. **Incremental Processing**
- Databricks Autoloader for efficient incremental file ingestion
- Processes only new/modified files, reducing compute costs
- Fault-tolerant with automatic checkpoint management

### 3. **Medallion Architecture**
- **Bronze Layer**: Raw data ingestion with minimal transformation
- **Silver Layer**: Cleaned, deduplicated, and validated data
- **Gold Layer**: Business-level aggregations and analytics-ready datasets

### 4. **Modular Notebook Design**
- Reusable Databricks notebooks with parameterization
- Orchestrated via Databricks Workflows
- Version-controlled and environment-agnostic

### 5. **Data Quality & Governance**
- Delta Live Tables for automated quality checks
- Schema evolution and enforcement
- Full data lineage tracking
- ACID compliance for reliable transactions



## Technologies Used

| Technology | Purpose |
|------------|---------|
| **Azure Data Factory** | Orchestration and data ingestion |
| **Azure Databricks** | Data processing and transformation |
| **Delta Lake** | Storage layer with ACID properties |
| **PySpark** | Distributed data processing |
| **Delta Live Tables** | Data quality and pipeline management |
| **Azure Data Lake Storage Gen2** | Scalable data storage |


## Azure Resources

### Resource Group
- **cm_netflix_project**: Container for all project resources

### Storage Account
- **cmnetflixprojectdl**: Azure Data Lake Storage Gen2 with containers:
  - `raw`: Staging area for source files
  - `bronze`: Raw Delta tables with full lineage
  - `silver`: Cleaned and validated Delta tables
  - `gold`: Business-ready aggregated tables

### Data Factory
- **cm-adf-netflixproject**: Orchestration service for data pipelines

### Databricks (Pending)
- Workspace to be created for transformation processing

**Architecture Pattern**: Medallion (Bronze → Silver → Gold)

For detailed setup instructions, see [docs/setup.md](docs/setup.md)

## Project Structure
```
├── adf/                          # Azure Data Factory artifacts
│   ├── pipelines/               # Pipeline JSON definitions
│   ├── datasets/                # Dataset configurations
│   └── linked_services/         # Connection configurations
│
├── databricks/                   # Databricks notebooks and configs
│   ├── bronze/                  # Bronze layer notebooks
│   ├── silver/                  # Silver layer notebooks
│   ├── gold/                    # Gold layer notebooks
│   └── utils/                   # Reusable utility functions
│
├── config/                       # Configuration files
│   └── pipeline_config.json     # Pipeline parameters
│
├── docs/                         # Additional documentation
│   ├── setup.md                 # Setup instructions
│   └── architecture.md          # Detailed architecture
│
└── README.md                     # This file
```


## Pipeline Workflow

### Phase 1: Ingestion (Bronze Layer)
1. ADF triggers on new file arrival or schedule
2. Validates source data availability and format
3. Databricks Autoloader incrementally ingests files
4. Raw data stored in Delta format with metadata

### Phase 2: Transformation (Silver Layer)
1. Data cleansing and deduplication
2. Schema validation and standardization
3. Business rule application
4. Data quality checks via Delta Live Tables

### Phase 3: Aggregation (Gold Layer)
1. Business-level transformations
2. Aggregations and metric calculations
3. Dimension and fact table creation
4. Analytics-ready datasets for consumption

Below is the high-level dataflow for this project. 
![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/8b09995291cf0e65d7e57b29535f9fd9deb12132/Designer.png)



## Key Learnings

- Implementing production-grade error handling in ADF pipelines
- Optimizing Databricks clusters for cost and performance
- Designing reusable, parameterized notebooks for scalability
- Managing Delta Lake table properties and optimization (OPTIMIZE, Z-ORDER)
- Implementing medallion architecture in real-world scenarios
- Orchestrating complex workflows with dependencies and parameters
- Applying data quality expectations with Delta Live Tables
- Building fault-tolerant, incremental processing pipelines


