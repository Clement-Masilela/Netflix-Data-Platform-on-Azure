# Utilities

## Purpose
Reusable helper functions and common utilities used across Bronze, Silver, and Gold notebooks.

## Modules
- `common_functions.py` - Shared transformation functions
- `config_loader.py` - Configuration and parameter management
- `logging_helper.py` - Standardized logging utilities
- `schema_definitions.py` - Common schema definitions

## Usage
These utilities promote code reusability and maintainability across the pipeline.
```python
# Example import in notebooks
from utils.common_functions import validate_schema, add_metadata
from utils.config_loader import load_pipeline_config
```
