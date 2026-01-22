# ADF Pipeline Walkthrough - Visual Guide

This document provides a step-by-step visual walkthrough of `p1_ingestion_pipeline` creation process.


## 1. Dataset Configuration - Dynamic Parameters

The source dataset uses parameterization to handle multiple files dynamically.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/dataset_github.png)


**Key Configuration**:
- Dataset: `ds_github`
- Parameter: `file_name` (String)
- Linked Service: `github_connection`
- No hardcoded file paths


## 2. Dynamic Content - Relative URL

The relative URL uses dynamic expressions to construct the full GitHub path at runtime.



**Expression**: `@dataset().file_name`

**Result**: Appends parameter value to base URL from linked service


## 3. Sink Dataset Configuration

The destination dataset is also parameterized for dynamic folder and file naming.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/dataset_sink.png)
**Key Configuration**:
- Dataset: `ds_sink`
- Container: `bronze` (browsed and selected)
- Parameters: 
  - `folder_name` (String)
  - `file_name` (String)
- Linked Service: `datalake_connection`

---

## 4. Pipeline Parameter Array

The pipeline uses an array parameter to define all files to be processed.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/p_array.png)

**Parameter Details**:
- **Name**: `p_array`
- **Type**: Array
- **Default Value**: JSON array with file mappings

**Array Structure**:
```json
[
  {
    "source_file": "netflix_titles.csv",
    "destination_folder": "titles",
    "destination_file": "netflix_titles"
  },
  {
    "source_file": "netflix_directors.csv",
    "destination_folder": "directors",
    "destination_file": "netflix_directors"
  },
  {
    "source_file": "netflix_countries.csv",
    "destination_folder": "countries",
    "destination_file": "netflix_countries"
  },
  {
    "source_file": "netflix_category.csv",
    "destination_folder": "categories",
    "destination_file": "netflix_category"
  },
  {
    "source_file": "netflix_cast.csv",
    "destination_folder": "cast",
    "destination_file": "netflix_cast"
  }
]
```

**Purpose**: Single configuration point for all files - add/remove files by editing this array.

## 5. ForEach Activity Configuration

The ForEach loop iterates through the parameter array.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/ForEach_Activvity.png)

**Configuration**:
- **Activity Name**: `ForAllTheFiles`
- **Items**: `@pipeline().parameters.p_array`
- **Sequential/Parallel**: [Specify based on your settings]

**Logic**: For each item in the array, execute the contained Copy Data activity.

---

## 6. Copy Activity Inside ForEach

The Copy Data activity is nested within the ForEach loop.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/ForEach_Activity2.png)

**Activity Name**: `Copy Github Data`

**Source Mapping**:
- Dataset: `ds_github`
- file_name parameter: `@item().source_file`

**Sink Mapping**:
- Dataset: `ds_sink`
- folder_name parameter: `@item().destination_folder`
- file_name parameter: `@item().destination_file`

**Runtime Behavior**: The `@item()` expression accesses the current array item during iteration.

---

## 7. Validation Activity Dataset

Validation activity ensures source files exist before processing.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/Validation.png)

**Activity Name**: `ValidationGithub`

**Purpose**: Pre-flight check to verify GitHub data availability

**Configuration**:
- Validates file existence
- Fails pipeline if source unavailable
- Prevents wasted compute on missing data


## 8. Validation Test Success

Testing the validation activity to ensure proper configuration.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/ValidationTest.png)

**Test Result**: Successful

**Verification**: Connection to GitHub established and files are accessible.


## 9. Complete Pipeline Canvas

The full pipeline showing all activities and their connections.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/pipeline_activity_flow.png)

**Pipeline Flow**:
1. **ValidationGithub** - Validates source data exists
2. **GithubMetadata** - Retrieves file metadata
3. **Set Variable** - Stores metadata
4. **ForAllTheFiles** - Iterates through file array
   - **Copy Github Data** - Copies each file to Bronze

**Dependencies**: Activities connected with "On Success" paths to ensure proper execution order.


## 10. Successful Pipeline Execution

Pipeline test run showing successful completion.

![image alt](https://github.com/Clement-Masilela/Netflix-Data-Platform-on-Azure/blob/main/docs/Screenshots/p1_ingestion_pipeline_canvas.png)

**Results**:
-  All activities succeeded
-  Validation passed
-  Metadata retrieved
-  ForEach loop completed all iterations
-  All 5 files copied to Bronze layer

**Execution Details**:
- Status: Succeeded
- Duration: 94 seconds
- Activities Run: 5+ (validation + metadata + variable + 5 copy activities)


## Key Takeaways

### Production-Grade Features Implemented

1. **Dynamic Parameterization**
   - No hardcoded values
   - Easy to maintain and extend
   - Environment-agnostic

2. **Validation First**
   - Fail-fast approach
   - Cost optimization
   - Error prevention

3. **Metadata Collection**
   - Audit trail capability
   - Data lineage tracking
   - Monitoring support

4. **Iterative Processing**
   - Scalable to many files
   - Single activity handles all copies
   - Reduces code duplication

5. **Reusable Datasets**
   - Parameter-driven design
   - Used across multiple activities
   - Separation of concerns



## Technical Highlights
 **ADF Best Practices**: Parameterization, validation, metadata-driven  
 **Dynamic Expressions**: `@item()`, `@dataset()`, `@pipeline()`  
 **Error Handling**: Pre-execution validation  
 **Scalability**: Easy to add more files  
 **Maintainability**: Configuration over code  


*This walkthrough demonstrates hands-on experience with Azure Data Factory and production-grade pipeline design.*
