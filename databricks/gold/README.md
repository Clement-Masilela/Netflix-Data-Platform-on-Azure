# Gold Layer

## Purpose
Business-level aggregations and analytics-ready datasets. Optimized for consumption by BI tools and data science workflows.

## Key Characteristics
- **Business metrics**: Aggregated KPIs and measures
- **Dimensional modeling**: Star/snowflake schema
- **Optimized for queries**: Partitioned and indexed appropriately
- **Production-ready**: Stable schemas for downstream consumption

## Notebooks
- `create_analytics_tables.py` - Fact and dimension table creation
- `aggregate_metrics.py` - Business metric calculations
- `build_data_marts.py` - Department-specific data marts

## Data Models
- **Fact Tables**: Viewing events, user activity
- **Dimension Tables**: Users, content, time
- **Aggregates**: Daily/weekly/monthly metrics

## Data Flow
```
Silver Tables → Business Logic → Gold Delta Tables
                      ↓
              Analytics & BI Tools
```

## Optimization
- Z-ORDER indexing on frequently filtered columns
- Partitioning by date for query performance
- OPTIMIZE operations scheduled regularly
