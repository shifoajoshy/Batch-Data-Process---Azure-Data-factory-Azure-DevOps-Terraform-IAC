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

---

## ETL Configuration
### Scope:
- Integrate OpenWeather REST APIs to enable seamless data extraction, transformation, and loading (ETL).
- Securely transfer data to Azure Blob Storage.
- Perform intermediate transformations to ensure data quality before final storage in Azure PostgreSQL.
- Implement robust authentication, data integrity checks, and monitoring systems.

---

## Conceptual Overview
- **Data Format Conversion**: Convert JSON data from OpenWeather APIs to CSV format.
- **Parquet File Transformation**: Transform CSV files to Parquet format for efficient ingestion into PostgreSQL.
- **Storage Optimization**: Delete intermediate CSV files post-transformation to save storage space.

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
#### Stage 1: Lookup and Dynamic API Calls
- Retrieve relative URLs from configuration files in Blob Storage.
- Use ForEach Activity to trigger Copy Activity for data ingestion.

#### Stage 2: Data Storage in Azure Data Lake
- Transfer API data to Blob Storage in JSON format.
- Convert JSON to CSV, then to Parquet for optimized storage.
- Separate pipelines for different regions (e.g., US and India).
[Include regional pipeline flow diagrams here]

#### Stage 3: Loading Data into PostgreSQL
- Ingest Parquet files from Azure Data Lake to PostgreSQL.
  - **Landing Schema**: Initial staging schema.
  - **Target Schema**: Production schema for analytics.
[Include schema diagrams here]

---

## Azure CI/CD Implementation
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
[Include CI/CD diagrams and steps here]

---

## Project and Repository Structure
- Centralized repository maintained in Azure DevOps for version control and collaboration.
- Structured to include:
  - Pipeline definitions.
  - Configuration files.
  - Deployment templates.
[Include repository structure diagrams here]

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
