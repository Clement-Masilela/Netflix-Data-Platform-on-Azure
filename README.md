# Netflix-Data-Platform-on-Azure
Production-grade Azure Data Engineering solution featuring dynamic ADF pipelines, Databricks Autoloader and Delta Lake with Medallion architecture

## Overview 
This project demonstrates a **production-grade, end-to-end data engineering solution on Azure**, designed for scalability, reusability and modern best practices. It covers ingestion, transformation, and modeling using **Azure Data Factory**, **Databricks**, and **Delta Lake** with **Medallion Architecture**.

## Key Features
- **Dynamic ingestion pipelines** in Azure Data Factory with parameterization and error handling.
- **Databricks Autoloader** for incremental file ingestion.
- **Reusable Databricks notebooks** orchestrated via Databricks Workflows.
- **Delta Lake with Medallion Architecture** (Bronze, Silver, Gold) for data quality and lineage.

## Tech Stack
- Azure Data Factory
- Azure Databricks
- Delta Lake
- Delta Live Tables


## Dataflow Architecture
Below is the high-level dataflow for this project. 
![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/8b09995291cf0e65d7e57b29535f9fd9deb12132/Designer.png)
