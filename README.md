# 🛒 E-commerce Customer Behavior — Medallion Architecture

This project demonstrates an **end-to-end data engineering pipeline** built using **Azure Databricks** and **Delta Lake**, following the **Medallion Architecture** pattern.  
The pipeline incrementally processes raw e-commerce data into analytics-ready datasets to support customer, product, and sales insights.

---

## 🏗️ Architecture Overview

![Project Architecture Diagram](images/diagram.png)

The solution is structured using **Bronze → Silver → Gold** layers:

### 🥉 Bronze — Raw Ingestion
- Ingests raw CSV data with **strict schema enforcement**
- Uses **watermark-based incremental ingestion** to avoid reprocessing
- Stores immutable raw data in **Delta Lake**
- Enables downstream incremental processing

### 🥈 Silver — Cleansed & Enriched Data
- Applies data quality checks and schema standardization
- Performs type casting and business rule validation
- Removes invalid records (missing keys, negative quantities)
- Deduplicates records and engineers analytical features
- Produces clean, analytics-ready datasets

### 🥇 Gold — Business Aggregations
- Builds business-level aggregate tables
- Uses **incremental merge-based updates**
- Avoids full recomputation of historical data
- Optimized for reporting and analytical workloads

---

## 📊 Pipeline Details

### 1️⃣ Ingestion & Environment Setup
- Mounted **Azure Data Lake Storage Gen2** containers using Service Principal authentication
- Defined explicit schemas to prevent schema drift during CSV ingestion
- Implemented incremental ingestion logic to ensure restart-safe processing

---

### 2️⃣ Transformation Layer (Silver)
- Enforced schema consistency with explicit type casting
- Handled missing values in critical business columns (`CustomerID`, `InvoiceNo`)
- Filtered out invalid transactions (negative quantities and prices)
- Engineered analytical features:
  - `TotalPrice`
  - Date and time attributes such as `Hour_of_day`, `Day_of_week`, `Month`, and `Year`

---

### 3️⃣ Aggregation Layer (Gold)
- **Customer Analytics**
  - Total spend per customer
  - Purchase frequency
  - First and last purchase timestamps
- **Product Performance**
  - Total quantity sold
  - Revenue contribution
  - Order frequency
- **Sales Trends**
  - Daily revenue and order counts grouped by country

Aggregations are designed to update incrementally to support scalable analytics.

---

## 🔄 Incremental Processing Strategy (High Level)
- Bronze ingestion applies **watermark-based filtering** to ingest only new data
- Downstream layers leverage **Delta Lake transactional guarantees**
- Merge-based writes ensure idempotency and eliminate destructive overwrites

This approach ensures the pipeline remains efficient, scalable, and production-ready.

---

## 🛠️ Tech Stack

| Category | Tools |
| :--- | :--- |
| **Cloud Platform** | ![Azure](https://img.shields.io/badge/azure-%230072C6.svg?style=for-the-badge&logo=microsoftazure&logoColor=white) |
| **Data Processing** | ![Databricks](https://img.shields.io/badge/databricks-%23FF3621.svg?style=for-the-badge&logo=databricks&logoColor=white) ![Apache Spark](https://img.shields.io/badge/Apache_Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white) |
| **Language** | ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![SQL](https://img.shields.io/badge/SQL-CC2927?style=for-the-badge&logo=postgresql&logoColor=white) |
| **Storage & Format** | ![Delta Lake](https://img.shields.io/badge/Delta_Lake-00ADEF?style=for-the-badge&logo=delta-lake&logoColor=white) |

---

## ✅ Key Engineering Highlights
- Medallion Architecture (Bronze / Silver / Gold)
- Incremental, restart-safe data processing
- Schema enforcement and data quality validation
- Analytics-ready Delta Lake tables
- Scalable aggregation design


