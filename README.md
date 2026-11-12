# CRM Data Health Assessment using Apache Spark & Machine Learning

An MSc Big Data Analytics project exploring CRM data quality, anomaly detection, and customer segmentation using **Apache Spark, PySpark, Isolation Forest, and K-Means**.

## Project Overview

Customer Relationship Management (CRM) datasets can contain missing values, inconsistent records, and unusual customer profiles.

This project uses the IBM Telco Customer Churn dataset to:
- Assess data quality through validation and preprocessing.
- Identify unusual customer profiles using Isolation Forest.
- Generate an anomaly-based Customer Health Score.
- Segment customers using K-Means clustering.

## Dataset

**IBM Telco Customer Churn Dataset**

| Attribute | Details |
|---|---|
| Customer records | 7,043 |
| Original columns | 21 |
| Engineered columns | 23 |
| Numerical ML features | 5 |
| Missing `TotalCharges` values | 11 |

The dataset includes customer demographics, services, contracts, billing, and churn information.

## Technology Stack

- Python
- Apache Spark
- PySpark
- Spark SQL
- Spark MLlib
- Scikit-learn
- Databricks
- Pandas
- Matplotlib

## Project Workflow

### 1. Data Preprocessing and Quality Assessment

- Loaded and explored the dataset using PySpark.
- Checked missing values across the dataset.
- Identified duplicate records and duplicate customer IDs.
- Validated categorical consistency across 17 categorical fields.
- Checked numerical ranges and billing values.
- Engineered two additional features: `LifetimeRevenue` and `CustomerStatus`.

**Key finding:** All 11 missing `TotalCharges` values belonged to customers with zero tenure. These values were replaced with 0 because the customers had not yet accumulated billing charges.

No exact duplicate records or duplicate customer IDs were found.

### 2. Anomaly Detection – Isolation Forest

Applied Scikit-learn's Isolation Forest to identify unusual numerical customer profiles.

**Features used:**
- SeniorCitizen
- Tenure
- MonthlyCharges
- TotalCharges
- LifetimeRevenue

**Model configuration:**
- Estimators: 100
- Contamination: auto
- Random state: 42

**Reported results:**

| Metric | Result |
|---|---:|
| Normal profiles | 4,239 |
| Unusual profiles | 2,804 |
| Anomaly rate | 39.81% |
| Average Health Score | 74.90 |

The Health Score is derived by rescaling the Isolation Forest decision function to a 0–100 range. A lower score indicates a more unusual numerical profile.

*Note: This is an anomaly-based score, not a measure of customer satisfaction, loyalty, or financial health.*

### 3. Customer Segmentation – K-Means

Implemented K-Means clustering using Spark MLlib with three clusters (`k=3`).

| Cluster | Customers | Avg. Tenure | Avg. Monthly Charges | Interpretation |
|---|---:|---:|---:|---|
| 0 | 4,177 | 18.2 months | 49.72 | New / Lower-value |
| 1 | 1,615 | 44.2 months | 77.86 | Regular / Medium-value |
| 2 | 1,251 | 64.4 months | 98.06 | Loyal / Higher-value |

The segment names are interpretations based on cluster characteristics, not predefined labels.

### 4. Model Evaluation

| Model | Evaluation | Reported result |
|---|---|---:|
| Isolation Forest | Anomaly rate | 39.81% |
| K-Means | Silhouette Score | 0.8002 |
| K-Means | WCSS | 7.40 × 10⁹ |

**Evaluation consideration:** The K-Means features have different numerical scales. `TotalCharges` and `LifetimeRevenue` can dominate distance calculations. Standardising features and reevaluating the clustering results is an important improvement.

## Key Outcomes

- Performed CRM data quality checks using PySpark.
- Identified and handled missing billing values.
- Applied unsupervised anomaly detection.
- Generated customer-level anomaly-based health scores.
- Segmented customers into three groups using Spark MLlib.
- Explored the use of distributed data processing alongside machine learning.

## Future Improvements

- Standardise numerical features before K-Means clustering.
- Reevaluate clustering performance after scaling.
- Validate anomaly detection using controlled anomaly injection.
- Explore additional CRM customer behaviour and retention features.
- Build an interactive dashboard for customer insights.

## Repository Contents

The repository contains the notebook, project documentation, and selected outputs.

## Disclaimer

This project was developed for academic and portfolio purposes as part of MSc Big Data Analytics. The dataset is used for analytical exploration and does not represent live business CRM records.

