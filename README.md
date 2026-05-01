# Real-Time Data Engineering Pipeline using Azure + Databricks

## Overview

This project is an end-to-end Data Engineering pipeline built using **Azure Data Factory, Azure Data Lake Storage (ADLS Gen2), Azure Event Hub, Databricks, PySpark Structured Streaming, and Delta Tables**.

The pipeline handles both:

* **Batch ingestion** of historical / lookup data
* **Real-time streaming ingestion** of ride events

Data is processed through Bronze, Silver, and Gold layers using a Medallion Architecture.

---

## Tech Stack

* Azure Data Factory
* Azure Data Lake Storage Gen2
* Azure Event Hub (Kafka)
* Azure Databricks
* PySpark
* Structured Streaming
* Delta Lake
* SQL

---

## Project Architecture

![Architecture](https://github.com/sathyasivamloganathan/Uber-DataEngineering-Project/blob/main/Images/architecture.png)

---

## Pipeline Flow

### 1. Batch Ingestion using Azure Data Factory

Azure Data Factory is used to ingest source JSON files into ADLS using Lookup + HTTP Ingestion activities.

Files loaded include:

* map_cities
* map_vehicle_types
* map_vehicle_makes
* map_payment_methods
* map_ride_statuses
* map_cancellation_reasons
* bulk_rides

![ADLS Pipeline](https://github.com/sathyasivamloganathan/Uber-DataEngineering-Project/blob/main/Images/ADLS.png)

---

### 2. Real-Time Streaming Ingestion

A Python event generator sends ride booking events to **Azure Event Hub**.

Databricks reads the stream using PySpark Structured Streaming and stores raw events in the Bronze layer.

---

### 3. Bronze Layer

Stores raw ingested data:

* rides_raw
* lookup tables
* bulk historical rides

---

### 4. Silver Layer

Streaming and batch data are cleaned, parsed, and joined with lookup tables to create:

* `stg_rides`
* `silver_obt` (One Big Table)

This layer prepares curated data for analytics.

---

### 5. Gold Layer

Final dimensional model built using Fact and Dimension tables.

### Dimension Tables

* dim_passenger
* dim_driver
* dim_vehicle
* dim_booking
* dim_location

### Fact Table

* fact_rides

### Slowly Changing Dimensions

* **SCD Type 1**
* **SCD Type 2**

---

## Databricks DAG Pipeline

Databricks pipeline automatically manages dependencies between Bronze → Silver → Gold layers.

![DAG Flow](https://github.com/sathyasivamloganathan/Uber-DataEngineering-Project/blob/main/Images/DAG-Flow.png)

---

## Folder Structure

```text
project/
│── Code_Files/
    │── Notebooks/
        ├── Bronze_ADLS.ipynb
        ├── Silver_Layer.ipynb
        ├── Gold Outputs.ipynb
    │── Pipelines/
        ├── ingest.py
        ├── silver.py
        ├── silver_obd.sql
        ├── model.py
```

## Key Learnings

* Building batch + streaming hybrid pipelines
* Kafka/Event Hub streaming ingestion
* Medallion Architecture design
* PySpark Structured Streaming
* STAR Schema modeling
* SCD Type 1 / Type 2 implementation
* Pipeline orchestration with Azure Data Factory and Databricks
