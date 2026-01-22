# Azure Data Factory Pipelines

## Overview
This directory contains the ADF pipeline configurations for the Netflix Data Platform. Pipelines are designed to be dynamic, parameterized, and production-ready with proper validation and error handling.


## Pipeline: Ingestion Pipeline

### Purpose
Dynamically ingest multiple CSV files from GitHub into the Bronze layer of Azure Data Lake Storage using parameterized datasets and iterative processing.

### Pipeline Name
`pl_ingestion` 


## Architecture

### High-Level Flow
```
Validation → Get Metadata → Set Variables → ForEach Loop → Copy Data
     ↓            ↓              ↓               ↓              ↓
  GitHub      File Info     Store Data    Iterate Files   GitHub → Bronze
```

### Detailed Activity Flow
```
1. ValidationGithub (Validation Activity)
   ↓ (On Success)
2. GithubMetadata (Web Activity)
   ↓ (On Success)
3. Set Variable Activity
   ↓ (On Success)
4. ForAllTheFiles (ForEach Loop)
   └── Copy Github Data (Copy Data Activity)
       ├── Source: GitHub (ds_github)
       └── Sink: ADLS Bronze (ds_sink)
```


## Activities Breakdown

### 1. ValidationGithub (Validation Activity)
**Purpose**: Ensure source files exist in GitHub before processing

- **Type**: Validation Activity
- **Dataset**: GitHub source dataset
- **Validation Logic**: Checks for file existence
- **On Success**: Proceeds to metadata retrieval
- **On Failure**: Pipeline stops, preventing unnecessary processing

**Why This Matters**: Prevents pipeline failures due to missing source data



### 2. GithubMetadata (Web Activity)
**Purpose**: Retrieve metadata about source files from GitHub

- **Type**: Web Activity
- **Method**: GET
- **URL**: [GitHub API endpoint](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/tree/main/data/raw)
- **Output**: File metadata (size, last modified, etc.)
- **Used For**: Logging and audit trail



### 3. Set Variable Activity
**Purpose**: Store metadata for downstream processing

- **Type**: Set Variable
- **Variable**: Stores GitHub metadata
- **Usage**: Available for logging, conditional logic, or monitoring



### 4. ForAllTheFiles (ForEach Loop)
**Purpose**: Iterate through multiple files dynamically

- **Type**: ForEach Activity
- **Sequential**: False
- **Items**: `@pipeline().parameters.p_array`

**Array Structure**:
```
[	
	{
		"folder_name":"netflix_cast",
		"file_name":"netflix_cast.csv"
	},
	{
		"folder_name":"netflix_category",
		"file_name":"netflix_category.csv"
	},
	{
		"folder_name":"netflix_countries",
		"file_name":"netflix_countries.csv"
	},
	{
		"folder_name":"netflix_directors",
		"file_name":"netflix_directors.csv"
	}
]
```

**Iteration Logic**: For each item in array, execute Copy Github Data activity



### 5. Copy Github Data (Copy Data Activity)
**Purpose**: Copy individual CSV file from GitHub to Bronze layer

**Source Configuration**:
- **Dataset**: `ds_github` (parameterized)
- **Linked Service**: `github_connection`
- **Relative URL**: `@item().source_file` (dynamic)
- **File Format**: DelimitedText (CSV)

**Sink Configuration**:
- **Dataset**: `ds_sink` (parameterized)
- **Linked Service**: `datalake_connection`
- **Container**: `bronze`
- **Directory**: `@item().destination_folder` (dynamic)
- **File Name**: `@item().destination_file` (dynamic)
- **File Format**: DelimitedText or Parquet

**Mapping**: Auto-mapping enabled (preserves source schema)


## Datasets

### ds_github (Source Dataset)
**Type**: Parameterized HTTP Dataset

- **Linked Service**: `github_connection`
- **Parameters**:
  - `file_name` (String): Dynamic file name from pipeline parameter
- **Relative URL**: `@dataset().file_name`
- **Format**: DelimitedText (CSV)
- **First Row as Header**: True

**Base URL Structure**:
```
https://raw.githubusercontent.com/[username]/[repo]/main/data/raw/{file_name}
```


### ds_sink (Destination Dataset)
**Type**: Parameterized Azure Data Lake Storage Gen2 Dataset

- **Linked Service**: `datalake_connection`
- **Parameters**:
  - `folder_name` (String): Target folder in Bronze container
  - `file_name` (String): Target file name
- **File Path**: 
  - Container: `bronze`
  - Directory: `@dataset().folder_name`
  - File: `@dataset().file_name`
- **Format**: DelimitedText or Parquet


## Pipeline Parameters

### p_array (Array)
**Type**: Array  
**Purpose**: Contains list of files to process with their configurations

**Default Value**: See array structure above

**Usage**: 
- Passed to ForEach activity
- Each item contains source and destination mapping
- Enables single pipeline to handle multiple files

**Benefits**:
- No hardcoding of file names
- Easy to add/remove files
- Supports different destination folders per file
- Metadata-driven approach


## Key Design Patterns

### 1. **Parameterization**
- Datasets use parameters instead of hardcoded values
- Pipeline uses array parameter for file list
- Enables reusability across environments

### 2. **Dynamic Content**
- Source and sink paths constructed at runtime
- Uses ADF expressions: `@item()`, `@dataset()`, `@pipeline()`
- Reduces code duplication

### 3. **Validation First**
- Pipeline validates data existence before processing
- Prevents wasted compute on missing data
- Fail-fast approach

### 4. **Metadata Collection**
- Captures source file information
- Enables audit trails and monitoring
- Supports data lineage tracking

### 5. **Iterative Processing**
- ForEach loop handles multiple files
- Can be configured for parallel or sequential processing
- Scalable to dozens of files


## Error Handling

### Built-in Mechanisms
- **Validation Activity**: Stops pipeline if source unavailable
- **Activity Dependencies**: "On Success" paths ensure proper flow
- **Copy Activity**: Built-in retry logic for transient failures

### Monitoring
- All activities log to ADF monitoring
- Failed runs can be investigated via pipeline runs view
- Activity outputs available for debugging


## Testing Results

### Initial Test Run
✅ **Status**: Succeeded  
✅ **Files Processed**: 5 CSV files  
✅ **Validation**: Passed  
✅ **Metadata Retrieval**: Successful  
✅ **ForEach Loop**: All iterations completed  
✅ **Data Copied**: All files landed in Bronze layer  

**Execution Time**: 94 seconds


## How to Run This Pipeline

### Manual Trigger
1. Navigate to ADF Author → Pipelines
2. Select the master ingestion pipeline
3. Click "Debug" or "Add Trigger → Trigger Now"
4. Pipeline uses default parameter array
5. Monitor execution in Monitor tab

### Scheduled Trigger (Future Enhancement)
- Configure schedule trigger for daily/hourly runs
- Time zone considerations for global data

### Event-Based Trigger (Future Enhancement)
- Trigger on new file arrival in GitHub
- Webhook-based execution



## Production Considerations

### Current State
- ✅ Dynamic and parameterized
- ✅ Validation logic implemented
- ✅ Reusable datasets
- ✅ Supports multiple files

### Future Enhancements
- [ ] Add error notification (email/Teams webhook)
- [ ] Implement retry policies
- [ ] Add schema validation
- [ ] Log to Azure Monitor/Log Analytics
- [ ] Implement incremental loading logic
- [ ] Add data quality checks post-copy



## Related Documentation
- [Linked Services](../linked_services/README.md)
- [Datasets Documentation](../datasets/README.md) (to be created)
- [Setup Guide](../../docs/setup.md)


*This pipeline demonstrates production-grade ADF design with dynamic parameterization, proper validation, and scalable architecture.*
