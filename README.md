****🛒 Azure End-to-End Data Engineering Project – Instacart Market Analytics****

****📌 Project Overview****

This project demonstrates end-to-end Azure Data Engineering solution built on the Instacart Market Analytics using a modern lakehouse architecture.

The goal of the project is to ingest data from multiple sources, process it using pyspark transformations and deliver data for analytics and business reporting.

The solution is designed using best practices such as metadata-driven ingestion, incremental data loading, parameterized pipelines, Delta Lake optimization, Slowly Changing Dimensions (SCD), workflow orchestration, and monitoring.

****🏗️ Architecture Overview:****

<img width="1767" height="762" alt="Instacart" src="https://github.com/user-attachments/assets/e5c1b14e-632c-418e-91da-de7586264589" />


**🎯 Key Design Principles:**

- Metadata Driven Ingestion Pipelines driven by metadata configuration, not hardcoded logic.
- Medallion architecture (Bronze – Silver – Gold) Lakehouse Pattern 
- Incremental Data Processing  
- Reusable & Parameterized Pipelines
- Fault Tolerance & Monitoring
- Optimized Data Modeling
  
****🧩 Ingestion Architecture (ADF)****

****🔹 Parent Pipeline****

- The parent pipeline controls ingestion dynamically using a metadata table.

  
  
**Activities:**

- Lookup Activity – Reads ingestion metadata from a database table
- ForEach Activity – Iterates over each metadata row
- Switch Activity – Routes execution based on source_type
- Execute Pipeline Activity – Executes source-specific child pipelines

**🔹 Child Pipelines by Source**

**1️⃣ Azure SQL Database Ingestion (Incremental)**

**Activities:**
- Lookup Activity – Reads CDC / watermark value
- Set Variable Activity – Stores current timestamp
- Copy Activity – Loads incremental data from Azure SQL DB to ADLS
- Script Activity – Captures maximum CDC column value
- Copy Activity – Updates watermark JSON file recursively

<img width="1919" height="960" alt="adf_pipeline" src="https://github.com/user-attachments/assets/3a0d0f2e-9a35-4244-a623-26f4ac99038e" />

**2️⃣ GitHub Ingestion**

****Activities:****

- Lookup Activity – Reads parameters file
- ForEach Activity – Iterates over file parameters
- Copy Activity – Copies data from GitHub to ADLS

**3️⃣ Azure Data Lake Ingestion**

**Activities:**

- Lookup Activity – Reads parameters file
- ForEach Activity – Iterates over configurations
- Copy Activity – Copies data within ADLS

****🥉 Bronze Layer****

- Raw data ingested from:
     1. Azure SQL Database
     2. GitHub
     3. Azure Data Lake
- Stored in Delta format
- No transformations applied
- Schema preserved as-is

****🥈 Silver Layer (Databricks)****

- Data reading from Bronze layer using PySpark
- Transformations performed:
     - Deduplication
     - Null handling
     - Data type casting
     - Column renaming and standardization
- Data written to Silver layer in Delta format
  
****Performance optimization:****

  - Delta table optimization

****🥇 Gold Layer****

  - Gold layer is designed using a star schema for BI consumption.

    
     <img width="1919" height="952" alt="Screenshot 2025-12-31 184235" src="https://github.com/user-attachments/assets/63d46f70-8ab0-4de7-a976-17202b066811" />

****Dimension Tables****

- dim_user
- dim_aisles
- dim_departments
- dim_products (SCD Type 2 implemented)
     
****Fact Tables****

- fact_orders
- fact_order_products
  
****Features:****

  - Surrogate keys
  - Historical tracking using SCD Type 2

****⚙️ Workflow Orchestration & Monitoring****

- Databricks Lakeflow Jobs used for:

  - Silver layer orchestration
  - Gold layer orchestration
  
- Job dependencies defined across notebooks

- Monitoring enabled:
   - Job execution status
   - Failure tracking
  
- Azure Logic Apps integrated for failure alert notifications

  
  <img width="1631" height="719" alt="Screenshot 2025-12-31 223348" src="https://github.com/user-attachments/assets/6dd70767-3ca5-4c66-97ef-bc9b1ed752a1" />


****📊 Power BI Integration****

- Delta tables registered in Databricks Metastore using SQL Editor
- Connected to Databricks SQL Warehouse
- Power BI dashboards built on gold-layer star schema
  

****🧰 Technologies Used****
   
- Cloud Platform:	Microsoft Azure
- Orchestration: Azure Data Factory
- Data Storage:	Azure Data Lake Gen2
- Processing Engine: Azure Databricks(Pyspark)
- Data Format: Delta Lake
- Database:	Azure SQL Database
- Metadata Management: SQL Metadata Tables
- Automation:	Azure Logic Apps
- Workflow:	Databricks Lakeflow Jobs
- Visualization: Power BI
- CI/CD: Azure DevOps
- Version Control: GitHub

****▶️ How to Run the Project****

- Deploy Azure resources (ADF, ADLS, Databricks, SQL DB).
- Populate metadata and parameter tables/files.
- Trigger ADF parent pipeline.
- Validate data ingestion into Bronze layer.
- Run Databricks Lakeflow jobs for:
- Silver transformations
- Gold modeling
- Validate Delta tables using Databricks SQL.
- Connect Power BI to SQL Warehouse and refresh dashboards.

****🌟 Key Highlights****
 
- Built a metadata-driven ingestion framework using Azure Data Factory
- Implemented incremental ingestion using CDC logic from Azure SQL DB
- Designed reusable and parameterized pipelines for multi-source ingestion
- Developed PySpark-based Silver transformations using Delta Lake
- Optimized Delta tables for performance
- Designed analytics-ready star schema with SCD Type 2 dimensions
- Orchestrated pipelines using Databricks Lakeflow Jobs
- Implemented monitoring and alerting using Azure Logic Apps
- Integrated Databricks with Power BI for enterprise reporting

****🏁 Conclusion****
 
  This project showcases an end-to-end Azure data engineering solution following industry-standard architecture and best practices.
  It demonstrates strong expertise in data ingestion, transformation, optimization, orchestration, and analytics delivery using the Azure ecosystem.
  The solution is scalable, maintainable, and extensible, making it suitable for real-world enterprise data platforms.
