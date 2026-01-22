# Pipeline Architecture

## Master Ingestion Pipeline Flow

### Visual Representation

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Ingestion%20pipeline%20architecture.png)

### Parameter Flow
```
Pipeline Parameter (p_array)
         ↓
    ForEach Activity
         ↓
    @item().source_file → ds_github.file_name → GitHub URL
    @item().destination_folder → ds_sink.folder_name → Bronze/folder
    @item().destination_file → ds_sink.file_name → filename
```

### Dynamic URL Construction

**Source (GitHub)**:
```
Base: https://raw.githubusercontent.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/tree/main/data/raw
Parameter: @dataset().file_name
Result: https://raw.githubusercontent.com/raw.githubusercontent.com/netflix_titles.csv
```

**Sink (ADLS)**:
```
Container: bronze
Directory: @dataset().folder_name
File: @dataset().file_name
Result: bronze/titles/netflix_titles
```

## Design Decisions

### Why ForEach Loop?
- **Scalability**: Easily add more files to p_array
- **Maintainability**: Single Copy activity handles all files
- **Flexibility**: Each file can have different destination structure

### Why Validation First?
- **Cost Optimization**: Don't run pipeline if source is unavailable
- **Fail Fast**: Immediate feedback on data issues
- **Resource Efficiency**: Prevents wasted compute

### Why Parameterization?
- **Reusability**: Same pipeline across dev/test/prod
- **Metadata-Driven**: Configuration changes don't require code changes
- **Enterprise Standard**: Industry best practice for production pipelines


## Execution Metrics

### Test Run Results
- **Total Files Processed**: 5
- **Success Rate**: 100%
- **Validation**:  Passed
- **Metadata Collection**:  Successful
- **Copy Activities**:  All succeeded


*This architecture represents production-grade data engineering practices*
