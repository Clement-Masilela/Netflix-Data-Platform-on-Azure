# Silver Layer

## Purpose
Cleaned, validated, and standardized data ready for business consumption. This layer applies data quality rules and transformations.

## Key Characteristics
- **Data cleansing**: Remove duplicates, handle nulls, fix data types
- **Schema enforcement**: Consistent schema across all tables
- **Business rules**: Apply domain-specific validation logic
- **Quality checks**: Delta Live Tables expectations

## Notebooks
- `transform_netflix_data.py` - Main transformation logic
- `data_quality_checks.py` - Quality validation and checks
- `deduplication.py` - Remove duplicate records

## Transformations Applied
- Standardize date formats
- Clean string fields (trim, case normalization)
- Type casting and validation
- Deduplication logic
- Business rule enforcement

## Data Flow
```
Bronze Tables → Transformations → Silver Delta Tables
                      ↓
              Quality Checks (DLT)
```
