# LANL Network Authentication Classification

## Overview

This project analyzes the **Los Alamos National Laboratory (LANL) Unified Host and Network Dataset** to classify enterprise authentication events as normal (Success) or anomalous (Fail). The analysis follows the **CRISP-DM methodology** and serves as an initial EDA and baseline modeling effort for network security monitoring.

## Dataset

- **Source**: [LANL Unified Host and Network Dataset](https://csr.lanl.gov/data/2017/)
- **Raw size**: ~73 GB (`auth.txt`, 1+ billion authentication events)
- **Sample**: 500,000 records via chunked reservoir sampling
- **Features**: 9 columns — Time*, SourceUser, DestUser, SourceComputer, DestComputer, AuthType, LogonType, Activity, Status
- **Note**: The Time column has been anonymized by the laboratory; temporal analysis is not possible.

## Key Findings

1. **Severe class imbalance**: Only ~1.2% of authentication events are failures (79:1 Success-to-Fail ratio).
2. **Behavioral features matter**: AuthType, LogonType, and Activity (low-cardinality) are more useful for encoding than identity features (10k–30k unique values).
3. **Logistic Regression and Decision Tree** achieved the best minority-class recall (~91%), catching most authentication failures, but at the cost of low precision (~13%).
4. **KNN** had the best precision (75%) but missed 96% of actual failures — unacceptable for security monitoring.
5. **SVM (RBF)** performed similarly to Logistic Regression but required significantly more compute time.
6. **Accuracy is misleading**: A naive baseline achieves 98.8% accuracy by predicting "Success" for every event, demonstrating why F1-Fail is the appropriate evaluation metric.

## Model Comparison

| Model | Recall (Fail) | Precision (Fail) | F1 (Fail) | Train Time |
|-------|--------------|------------------|-----------|------------|
| Baseline (Most Frequent) | 0.00 | 0.00 | 0.00 | 0.08s |
| Logistic Regression | 0.91 | 0.13 | 0.22 | 1.33s |
| Decision Tree | 0.91 | 0.13 | 0.22 | 0.49s |
| KNN (k=5) | 0.04 | 0.75 | 0.08 | 0.16s |
| SVM RBF (50k subsample) | 0.90 | 0.13 | 0.22 | 8.90s |

## Notebook

**[LANL_Auth_Classification.ipynb](LANL_Auth_Classification.ipynb)** — Full analysis including:
- Data cleaning (missing values, duplicates, cardinality assessment)
- EDA with visualizations (class imbalance, feature distributions, correlation heatmap, outlier analysis)
- Feature engineering (OneHotEncoding, user behavioral profiles)
- Baseline and 4 classification models with standardized evaluation
- Confusion matrix visualizations and metric comparison charts

## Next Steps

- Ensemble methods (Random Forest, Gradient Boosting) for improved imbalanced-class performance
- Anomaly detection framing (Isolation Forest, One-Class SVM)
- Stratified k-fold cross-validation for more robust estimates
- Decision threshold tuning based on business cost of false positives vs. false negatives

## Repository Structure

```
Network_Classification/
├── README.md                           # This file
├── LANL_Auth_Classification.ipynb      # Main analysis notebook
└── Dataset_Lab.txt                     # Dataset description
```
