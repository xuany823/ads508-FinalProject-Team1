# GlowSense Beauty Analytics
### Predicting Product Success in the Beauty Industry

> **ADS-508: Data Science With Cloud Computing** | University of San Diego  
> Team 1 · March – April 2026

---

## 👥 Team

| Name | GitHub |
|------|--------|
| Jee Hun Hwang | [@jimmyhwang](https://github.com/jimmyhwang) |
| Lei Lin | [@leilin](https://github.com/leilin) |
| Michelle (Xuan) Wang | [@xuany823](https://github.com/xuany823) |

---

## 📌 Project Overview

GlowSense Analytics is a data science startup providing machine learning-driven insights for beauty retailers and brands. This project analyzes product attributes and over one million customer reviews from the Sephora online marketplace to identify factors that influence product success.

Using predictive modeling and natural language processing, GlowSense forecasts product ratings, analyzes customer sentiment, and identifies product characteristics associated with strong market performance — helping retailers make smarter decisions around product assortment, marketing strategy, and inventory planning.

---

## 🧩 Problem Statement

Thousands of beauty products launch on Sephora every year — most fail to gain traction. Retailers waste budgets and inventory on underperforming products without data-driven foresight, because they have no reliable way to predict what customers will love.

GlowSense solves this by predicting product success **before launch** using ML & NLP on real customer signals from Sephora's marketplace.

---

## 🎯 Goals

| Goal | Description |
|------|-------------|
| **Predict** | Forecast which new Sephora products will achieve high customer ratings and strong popularity |
| **Prescribe** | Identify the product attributes (price, ingredients, brand, sentiment) that drive success |
| **Inform** | Surface performance trends and customer preferences to support better retail decisions |

### Out of Scope
- Product formulation or ingredient recommendations
- Automated inventory or pricing decisions
- Individual customer personalization / recommendation systems

---

## 📂 Repository Structure

```
ads508-FinalProject-Team1/
│
├── Notebooks/
│   ├── sephora_pipeline.ipynb       # Data ingestion & S3 upload pipeline
│   ├── sephora-eda.ipynb            # Exploratory data analysis
│   ├── feature_engineering.ipynb    # Feature creation & transformation
│   ├── modeling_products.ipynb      # Product-based regression models
│   └── modeling_reviews.ipynb       # Review-based classification models
│
├── data/
│   └── README.md                    # Data access instructions (files stored in S3)
│
├── docs/
│   ├── Assignment2_1_Team1.docx     # Design document Part 1
│   ├── Assignment3_1_Team1.docx     # Data exploration
│   ├── Assignment4_1_Team1.docx     # Data preparation & feature engineering
│   ├── Assignment5_1_Team1.docx     # Model training
│   └── Assignment6_1_Team1.docx     # Future enhancements
│
└── README.md
```

---

## 🗄️ Data Source

**Source:** [Kaggle — Sephora Products and Skincare Reviews](https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews)

| Dataset | Rows | Columns | Description |
|---------|------|---------|-------------|
| `product_info.csv` | 8,494 | 27 | Product metadata — brand, price, category, ingredients |
| `reviews_0-250.csv` | ~250K | 18 | Customer reviews with ratings & text |
| `reviews_250-500.csv` | ~250K | 18 | |
| `reviews_500-750.csv` | ~250K | 18 | |
| `reviews_750-1250.csv` | ~500K | 18 | |
| `reviews_1250-end.csv` | ~94K | 18 | |

**Total: 8,494 products · 1,094,411 reviews**

---

## ☁️ Cloud Architecture (AWS)

```
Kaggle Dataset
     │
     ▼
┌─────────────────────────────────────────┐
│         Amazon S3 (ads508-team1-sephora) │
│                                          │
│  raw/products/product_info.csv           │
│  raw/reviews/reviews_*.csv               │
│                  │                       │
│                  ▼                       │
│  curated/products/products.parquet       │
│  curated/reviews/reviews.parquet         │
│                  │                       │
│  analytics/query_results/                │
│  athena/staging/                         │
└─────────────────────────────────────────┘
     │                        │
     ▼                        ▼
Amazon SageMaker Studio    Amazon Athena
(EDA, training, modeling)  (SQL queries on curated data)
```

**S3 Bucket:** `ads508-team1-sephora`  
**Instance:** `ml.m5.xlarge` (CPU-based, single instance)  
**Approach:** Bring-your-own-script via SageMaker managed training

---

## 🔧 Feature Engineering

### Product Model (~190 features total)

| Feature | Description |
|---------|-------------|
| `popularity_index` | `reviews × rating` — captures highly rated & widely reviewed products |
| `price_bucket` | Bucketed into `budget / low_mid / mid / premium` |
| `log_loves` | `log1p(loves_count)` — reduces skew from viral products |
| `num_ingredients` | Count of ingredients per product |
| `ing_*` (×100) | TF-IDF binary indicators for top 100 ingredients (e.g., `ing_squalane`, `ing_glycerin`) |
| `secondary_category` | One-hot encoded |
| `brand_group` | One-hot encoded |
| `price_usd` | StandardScaler normalized |

### Review Model

| Feature | Description |
|---------|-------------|
| TF-IDF (unigrams + bigrams) | Text vectorization of `review_text` |
| `vader_pos / neg / neu / compound` | VADER sentiment scores |
| `rating` | Target variable (classification: high ≥ 4.0 vs. low) |

**Excluded:** `product_id`, `product_name`, `brand_id`, `brand_name` (high-cardinality identifiers)

---

## 🤖 Models

### Review-Based Classification (Predict rating: high vs. low)

| Model | Accuracy | F1 (Macro) |
|-------|----------|------------|
| SGDClassifier | 0.728 | 0.503 |
| Logistic Regression | 0.714 | 0.477 |
| Linear SVM | 0.684 | 0.490 |

**Best tuned Logistic Regression** (RandomizedSearchCV + RepeatedStratifiedKFold):
- Best Score: **0.952**
- Hyperparameters: `C=0.00564, penalty=l2, solver=lbfgs`

### Product-Based Regression (Predict rating score)

| Model | R² | RMSE |
|-------|-----|------|
| XGBoost | 0.368 | 0.470 |
| Random Forest | 0.322 | 0.487 |
| Linear Regression | 0.240 | 0.515 |

**Hyperparameter tuning:** RandomizedSearchCV / GridSearchCV  
**Data split:** 90% train · 5% validation · 5% test

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **Amazon S3** | Data lake — raw & curated layers |
| **Amazon SageMaker Studio** | Notebook environment, model training |
| **Amazon Athena** | SQL querying on curated Parquet data |
| **Python / pandas / pyarrow** | Data processing |
| **scikit-learn** | Feature engineering, modeling, tuning |
| **XGBoost** | Gradient boosted model (SageMaker built-in) |
| **VADER** | Sentiment scoring on review text |
| **TF-IDF / CountVectorizer** | Text & ingredient feature extraction |
| **seaborn / matplotlib** | EDA visualization |

---

## 🔒 Security & Privacy

| Concern | Status |
|---------|--------|
| PHI (Protected Health Information) | ❌ Not present |
| PII (Personally Identifiable Information) | ❌ Not present — `author_id` is anonymized |
| Credit card data | ❌ Not present |
| User behavior tracking | ✅ Indirect only — existing public review fields |
| S3 access | Read from `raw/` · Write to `curated/` · Optional: `athena/staging/` |

---

## ⚠️ Known Limitations & Bias

- **Positivity bias:** Most Sephora ratings skew toward 4–5 stars
- **Popularity bias:** High-review products dominate engagement metrics (`loves_count`, review volume)
- **Product-review join issue:** Inconsistent `product_id` values prevent reliable join between datasets — reviews are treated as a standalone signal source
- **Demographic bias:** Attributes like `skin_tone` and `skin_type` are partially missing

---

## 🚀 Future Enhancements

1. **Transformer-based embeddings** — Replace TF-IDF + SVD with BERT for richer ingredient and review text representations
2. **Improved product-review integration** — Build a fuzzy matching system using product names and brand similarity to enable unified feature sets
3. **Expanded dataset & ensemble modeling** — Incorporate additional product data to improve product-level model generalization; explore stacking and ensemble methods

---

## 📎 Links

- **GitHub Repository:** [https://github.com/xuany823/ads508-FinalProject-Team1](https://github.com/xuany823/ads508-FinalProject-Team1)
- **Ingestion Notebook:** [sephora_pipeline.ipynb](https://github.com/xuany823/ads508-FinalProject-Team1/blob/Michelle/sephora_pipeline.ipynb)
- **EDA Notebook:** [sephora-eda.ipynb](https://github.com/xuany823/ads508-FinalProject-Team1/blob/Jimmy/sephora-eda.ipynb)
- **Data Source:** [Kaggle — Sephora Products & Skincare Reviews](https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews)

---

*GlowSense Beauty Analytics · ADS-508 Team 1 · University of San Diego · 2026*
