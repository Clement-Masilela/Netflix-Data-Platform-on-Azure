# Azure Data Factory Pipelines

## Purpose
Orchestration and ingestion pipeline definitions for the data platform.

## Pipelines
- `master_ingestion_pipeline.json` - Main orchestration pipeline
- `validate_and_ingest.json` - Data validation and ingestion logic
- `trigger_databricks_workflows.json` - Databricks job orchestration

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
