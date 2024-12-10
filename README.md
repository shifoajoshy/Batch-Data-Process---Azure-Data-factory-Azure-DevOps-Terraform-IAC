# Data Pipeline for OpenWeather REST APIs

## Objective
Design and implement a data pipeline capable of extracting large volumes of data from REST APIs that do not support pagination.

---

## REST APIs Used
- **[Current Weather Data API](https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid={API key})**
- **[Air Pollution Data API](http://api.openweathermap.org/data/2.5/air_pollution?lat={lat}&lon={lon}&appid={API key})**

---

## Logical Components Utilized
- **Azure Data Factory**: Orchestrates data movement and transformation.
- **Azure DevOps**: Facilitates version control, CI/CD pipeline setup, and collaboration.
- **Azure Data Lake**: Provides secure and scalable storage for intermediate and raw datasets.
- **Azure PostgreSQL**: Serves as the target database for structured and processed data storage.
---

## Architecture
![Architecture](https://github.com/user-attachments/assets/9da270af-547c-4a57-b78a-9058174642bf)

## Logical Flow
![logical flow](https://github.com/user-attachments/assets/024e5c4d-ac39-4c1c-bcd3-ebf65832adf0)

---

## ETL Configuration
### Scope:
- Integrate OpenWeather REST APIs to enable seamless data extraction, transformation, and loading (ETL).
- Securely transfer data to Azure Blob Storage.
- Perform intermediate transformations to ensure data quality before final storage in Azure PostgreSQL.
- Implement robust authentication, data integrity checks, and monitoring systems.

---

## Activities Used
- **Web Activity**: Connects to OpenWeather APIs and retrieves JSON data.
- **Azure Key Vault**: Secures storage and retrieval of API keys.
- **ForEach Activity**: Iterates over latitude and longitude values dynamically.
- **Lookup Activity**: Fetches API endpoint configurations from Blob Storage.
- **Condition Activity**: Validates row counts of read and written data.
- **Copy Activity**: Transfers data between REST APIs, Azure Blob Storage, and PostgreSQL.

---

## Pipeline Configuration
### Pipeline Frequency:
- Scheduled to run daily on business days.

### Stages of Azure Data Factory Pipelines:

#### Conceptual Overview
- **Data Format Conversion**: Convert JSON data from OpenWeather APIs to CSV format.
- **Parquet File Transformation**: Transform CSV files to Parquet format for efficient ingestion into PostgreSQL.
- **Storage Optimization**: Delete intermediate CSV files post-transformation to save storage space.
  
#### Main pipeline
![Main pipeline](https://github.com/user-attachments/assets/ddcdbc03-7299-4fd4-8bec-44ce4e2f77e4)

#### Stage 1: Lookup and Dynamic API Calls
- Retrieve relative URLs from configuration files in Blob Storage.
- Use ForEach Activity to trigger Copy Activity for data ingestion.
  ![foreach](https://github.com/user-attachments/assets/bebb5a71-14f2-447d-a570-579a6aee17f2)

#### Stage 2: Data Storage in Azure Data Lake
- Transfer API data to Blob Storage in JSON format.
- Convert JSON to CSV, then to Parquet for optimized storage.
- Separate pipelines for different regions.
  
##### For Extracting US region data
![stage 2](https://github.com/user-attachments/assets/a0f5199e-e22f-4534-9d4c-0dc3ede8d990)
##### For Extracting India region data
![stage 2 india](https://github.com/user-attachments/assets/9d2957c4-32b6-4ed0-a90c-35c70c0e1073)


#### Stage 3: Loading Data into PostgreSQL
![schemas](https://github.com/user-attachments/assets/7b535fc4-4690-48bf-9b70-efeab3cf0d2b)
- Ingest Parquet files from Azure Data Lake to PostgreSQL.
  - **Landing Schema**: Initial staging schema.
    ![land](https://github.com/user-attachments/assets/a50fd17f-3d33-448b-a696-ac8da352dd6b)
  - **Target Schema**: Production schema for analytics.
    ![target](https://github.com/user-attachments/assets/d0002b2a-8844-4ae8-bb94-0bf01b4086df)
    
---

## Azure CI/CD Implementation
![artifacts](https://github.com/user-attachments/assets/b439d094-e659-4790-a257-c4df84fee1ba)
### Key Actions:
- **Pre-Deployment**:
  - Stop triggers.
  - Update global parameters.
  - Validate pipeline configurations.
- **Post-Deployment**:
  - Start triggers.
  - Monitor pipeline performance.
  - Clean up removed resources.
- Deployment workflows for Pre-Production and Production environments are systematically managed.
![prepost deploment](https://github.com/user-attachments/assets/386033a3-765a-414d-840e-10de64793546)

---

## Project and Repository Structure
- Centralized repository maintained in Azure DevOps for version control and collaboration.
- Structured to include:
  - Pipeline definitions.
  - Configuration files.
  - Deployment templates.
    
![sucess deploy](https://github.com/user-attachments/assets/af79d32b-1c3d-4095-90ab-bcfb75ff86c9)

---

## Best Practices Implemented
1. **Parallel Pipeline Execution**: Run pipelines concurrently to reduce processing time.
2. **Monitoring and Alerts**: Logic Apps and database logging track pipeline runs and send failure alerts.
3. **Error Handling**: Retry policies and logging ensure minimal downtime and quick debugging.
4. **Dynamic Configuration**: Global parameters enable flexible and reusable pipelines.
5. **Secure Key Management**: API keys are securely stored in Azure Key Vault.
6. **Storage Optimization**: Convert intermediate files to Parquet and delete temporary files.
7. **Scalable Design**: Architecture supports increased data loads and new APIs.
8. **Data Validation**: Row-level checks maintain data integrity.
9. **Structured Logging**: Logs provide an audit trail for troubleshooting.

---
