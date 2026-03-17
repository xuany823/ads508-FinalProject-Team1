# ads508-FinalProject-Team1

## S3 Data Lake Structure

The project uses an Amazon S3 bucket (`ads508-team1-sephora`) organized following a layered data lake architecture to ensure data lineage, scalability, and efficient processing.
- **raw/**  
  Stores original, unmodified datasets to preserve data lineage and ensure reproducibility.

- **curated/**  
  Contains cleaned and transformed datasets in Parquet format, optimized for efficient querying and analytics.

- **analytics/**  
  Stores query outputs and derived datasets generated during analysis.

- **athena/**  
  Used for Athena query staging and temporary files during SQL execution.

This layered architecture supports scalable data processing, cost-efficient querying with Amazon Athena, and a clear separation between raw and processed data.

ads508-team1-sephora/
│
├── raw/                          # Immutable raw data (source of truth)
│   ├── products/
│   │   └── product_info.csv
│   │
│   └── reviews/
│       ├── reviews_0-250.csv
│       ├── reviews_250-500.csv
│       ├── reviews_500-750.csv
│       ├── reviews_750-1250.csv
│       └── reviews_1250-end.csv
│
├── curated/                      # Cleaned & processed datasets (Parquet format)
│   ├── products/
│   │   └── products.parquet
│   │
│   └── reviews/
│       └── reviews.parquet
│
├── analytics/                    # Query outputs and derived datasets
│   └── query_results/
│
└── athena/                       # Athena query staging and temporary files
    └── staging/
