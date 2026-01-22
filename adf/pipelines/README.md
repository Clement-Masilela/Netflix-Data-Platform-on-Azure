# Azure Data Factory Pipelines

## Purpose
Orchestration and ingestion pipeline definitions for the data platform.

## Pipelines
- [p1_ingestion.json](../pipelines/p1_ingestion.json) - Main orchestration pipeline (Complete with validation)
- `trigger_databricks_workflows.json` - Databricks job orchestration (To be completed)

## Features
- **Parameterization**: Dynamic pipeline execution
- **Error handling**: Retry logic and failure notifications
- **Metadata-driven**: Configuration-based execution
- **Scheduling**: Time-based and event-based triggers

## Parameters
Common parameters used across pipelines:
- `source_path`: Source data location
- `target_container`: Destination container
- `processing_date`: Date partition for processing
