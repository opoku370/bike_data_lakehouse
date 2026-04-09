# End-to-End Lakehouse Data Pipeline (Medallion Architecture)
End-to-end Data Lakehouse project using Databricks and Medallion Architecture (Bronze, Silver, Gold) to transform raw bike data into analytics-ready datasets.

## Overview

This project implements an end-to-end **data engineering pipeline** using the **Medallion Architecture (Bronze, Silver, Gold)** on Databricks. It demonstrates how raw data from multiple sources can be transformed into **analytics-ready datasets** for business intelligence and reporting.

The pipeline integrates CRM and ERP datasets, applies transformations using **Apache Spark**, and models the data into a **star schema** optimized for analytical workloads.

---

## Objectives

- Design a scalable data pipeline architecture  
- Clean and integrate multi-source data (CRM & ERP)  
- Build dimension and fact tables  
- Enable downstream analytics and reporting  
- Automate workflows using notebook orchestration  

---

## Architecture

``` Bronze Layer  →  Silver Layer  →  Gold Layer ```

### Bronze Layer

- Stores raw ingested data
- No transformations applied
- Serves as the source of truth

### Silver Layer

- Data cleansing and standardization
- Integration of CRM and ERP datasets
- Handling of missing and inconsistent data

### Gold Layer

- Business-level data modeling which is optimized for analytics and BI tool

---

## Tech Stack

- Databricks  
- Apache Spark (PySpark & Spark SQL)  
- Delta Lake  
- Python  
- SQL  

---

## Data Modeling

The Gold layer implements a **star schema**:

### Dimension Tables
- `dim_customers`: Customer attributes  
- `dim_products`: Product and category details  

### Fact Table
- `fact_sales`:
  - Measures: `sales_amount`, `quantity`, `price`  
  - Foreign Keys: `customer_key`, `product_key`  
  - Dates: `order_date`, `ship_date`, `due_date`  

Surrogate keys are generated using window functions:

```sql
ROW_NUMBER() OVER (ORDER BY start_date, product_number) AS product_key
```

---

## Data Storage

All tables are stored in Delta Lake format:

``` df.write.mode("overwrite").format("delta").saveAsTable("schema.table") ```

### Benefits

- ACID transactions
- Schema enforcement
- Time travel capabilities
- Scalable data storage

---

## Pipeline Orchestration


<img width="1005" height="140" alt="pipeline" src="https://github.com/user-attachments/assets/ec116575-1b81-4ad0-9395-1d167565252f" />

Workflows are orchestrated using Databricks notebooks:

- Bronze Layer Orchestration
  - Executes the notebook for the raw datasets
  
- Silver Layer Orchestration
  - Executes all transformation notebooks for cleaned datasets

- Gold Layer Orchestration
  - Builds dimension and fact tables

Execution is automated using:

``` python
dbutils.notebook.run(notebook_path, timeout_seconds=0)
```

---

## Project Structure
<img width="298" height="446" alt="structure" src="https://github.com/user-attachments/assets/b082c223-63cd-4828-907f-ff95c8aceac4" />



---

## Use Cases
- Sales performance analysis
- Customer behavior analysis
- Product performance tracking
- Business intelligence dashboards (Power BI, Tableau)

---

## Key Highlights
- End-to-end pipeline from raw data to analytics-ready datasets
- Implementation of Medallion Architecture
- Scalable data processing using Spark
- Structured data modeling with star schema
- Modular and reusable notebook orchestration





