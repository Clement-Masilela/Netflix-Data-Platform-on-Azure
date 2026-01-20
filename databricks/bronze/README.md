# Bronze Layer

## Purpose
Raw data ingestion layer with minimal transformation. This layer stores data in its original format with added metadata for lineage tracking.

## Key Characteristics
- **Append-only**: Full historical data retention
- **Minimal transformation**: Data stored as-is from source
- **Metadata enrichment**: Ingestion timestamps, source file information
- **Delta format**: ACID compliance for reliability

## Notebooks
- `ingest_netflix_data.py` - Main ingestion notebook using Autoloader
- `validate_source_data.py` - Pre-ingestion validation checks

## Data Flow
```
Source Files → Autoloader → Bronze Delta Tables
```

## Schema
Bronze tables maintain source schema with additional columns:
- `_ingestion_timestamp`: When data was loaded
- `_source_file`: Original file path
- `_processing_batch_id`: Batch identifier for tracking
