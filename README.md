# Sephora Product Analytics Pipeline (AWS MLOps)

## Project Overview

This project builds an end-to-end **cloud-based data pipeline and MLOps workflow** on AWS to analyze Sephora product and customer review data. The system enables scalable data ingestion, transformation, exploration, and machine learning experimentation to identify key drivers of product success and customer sentiment.

The architecture follows a **modern data lake design**, separating raw, curated, and analytics layers to ensure data quality, reproducibility, and efficient querying.

---

## Business Objective

The goal of this project is to:

- Identify key factors driving **product ratings and customer satisfaction**
- Enable **data-driven product selection and marketing decisions**
- Support scalable analytics and machine learning workflows in a cloud environment

---

## Architecture Overview

The pipeline is built using AWS services:

- **Amazon S3** → Data lake storage (raw, curated, analytics)
- **Amazon SageMaker** → Data processing, feature engineering, ML workflow
- **Amazon Athena** → Serverless SQL querying on S3
- **GitHub** → Version control and collaboration

---

## AWS Architecture Diagram

            +----------------------+
            |   Raw Data (CSV)     |
            |  (Products, Reviews) |
            +----------+-----------+
                       |
                       v
            +----------------------+
            |   Amazon S3 (Raw)    |
            |   /raw/              |
            +----------+-----------+
                       |
                       v
            +----------------------+
            |  SageMaker Notebook  |
            |  - Data Cleaning     |
            |  - Feature Eng.      |
            +----------+-----------+
                       |
                       v
            +----------------------+
            |  Amazon S3 (Curated) |
            |  /curated/ (Parquet) |
            +----------+-----------+
                       |
            +----------+-----------+
            |                      |
            v                      v
    +---------------+      +------------------+
    |  Athena (SQL) |      |   ML Workflow    |
    |  Query Engine |      |  (SageMaker)     |
    +-------+-------+      +--------+---------+
            |                       |
            v                       v
    +----------------+     +------------------+
    | Analytics Data |     | Models & Insights|
    | /analytics/    |     | Predictions      |
    +----------------+     +------------------+

    ---

## Data Pipeline

### 1. Data Ingestion
- Raw CSV files are uploaded to **S3 raw layer**
- Data is read into **SageMaker notebooks**

### 2. Data Processing
- Data cleaning and validation
- Merging multiple review files
- Feature engineering

### 3. Data Transformation
- Converted into **Parquet format**
- Stored in **S3 curated layer**

### 4. Data Access
- Queried using **Amazon Athena**
- Supports scalable SQL-based analytics

### 5. Machine Learning (MLOps)
- Feature engineering pipeline
- Model experimentation and evaluation
- Reproducible workflows in SageMaker

---

## Tools & Technologies

- **Programming:** Python (pandas, numpy)
- **Cloud:** AWS S3, SageMaker, Athena
- **Data Format:** CSV, Parquet
- **Visualization:** matplotlib, seaborn
- **Version Control:** GitHub

---

## Key Capabilities

- Scalable **ETL pipeline on AWS**
- Structured **data lake architecture**
- Efficient **SQL analytics on large datasets**
- Reproducible **MLOps workflow**
- Feature engineering for predictive modeling

---

## Evaluation Metrics

- Model performance: Accuracy, Precision, Recall, F1-score  
- Feature importance analysis for product success  
- Sentiment analysis performance on review text  

---

## Repository

- GitHub:  
  https://github.com/xuany823/ads508-FinalProject-Team1  

- Data Ingestion:  
  https://github.com/xuany823/ads508-FinalProject-Team1/blob/Michelle/sephora_pipeline.ipynb  

- EDA:  
  https://github.com/xuany823/ads508-FinalProject-Team1/blob/Jimmy/sephora-eda.ipynb  

---

## Future Improvements

- Add automated pipeline orchestration (AWS Step Functions / Airflow)
- Deploy models for real-time inference
- Integrate advanced NLP models for sentiment analysis
- Build dashboard for business users

---

## Summary

This project demonstrates how a **modern AWS-based data lake and MLOps pipeline** can support scalable analytics and machine learning workflows, enabling data-driven decision-making in an e-commerce environment.
