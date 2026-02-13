# Berkeley_Capstone_2026_NetworkLogs
Berkeley Capstone Project

___

# Network Authentication Classification Analysis  
**Following CRISP-DM Methodology and ML Reference Guide**

---

## Project Overview

### Problem Statement
Because authentication failures are rare and context-dependent, we reframed the problem from predicting failures to modeling **normal authentication behavior**.

Using the CRISP-DM methodology, we first conducted exploratory and information-theoretic analysis to determine which features encode **behavior** versus **identity**. This allowed us to design aggregated, window-based features aligned with enterprise authentication processes, enabling robust anomaly detection rather than brittle classification.

---

## Refined Research Question

Can we effectively distinguish between normal and anomalous enterprise authentication patterns by modeling **behavioral features** (authentication type, logon type, activity orientation) rather than identity features?  

Additionally, which classification techniques best capture these patterns under severe class imbalance (~1.2% failure rate)?

---

## Data Source

**Dataset:** Los Alamos National Laboratory (LANL) Unified Host and Network Dataset  
**File:** `auth.txt` (~73GB, 1+ billion authentication events)  
**Sample Size:** 500,000 stratified random samples using reservoir sampling  

**Structure (9 columns):**
- `Time*`
- `SourceUser`
- `DestUser`
- `SourceComputer`
- `DestComputer`
- `AuthType`
- `LogonType`
- `Activity`
- `Status`

> *Note:* The `Time` column is anonymized/classified. Temporal sequence analysis was not possible.

---

## Methodology & Techniques

### Feature Engineering
- One-Hot Encoding for low-cardinality behavioral features:
  - `AuthType`
  - `LogonType`
  - `Activity`

### Identity-Based Aggregation
- Behavioral profiles per user/computer  
- Alternative to temporal sliding-window modeling

### Dimensionality Reduction
- Principal Component Analysis (PCA) to understand feature structure and relationships

### Classification Models
- Logistic Regression
- Decision Trees
- K-Nearest Neighbors (KNN)
- Support Vector Machines (SVM)
- Class-weight balancing to address severe imbalance

### Evaluation Metrics
- Precision
- Recall
- F1-score  
(Focus on minority-class performance)

---

## Expected Results

- Identification of behavioral patterns distinguishing successful vs failed authentications  
- Standardized model performance comparison  
- Feature importance ranking  
- Anomaly detection baseline for enterprise monitoring  

---

## Why This Question Matters

Enterprise authentication logs represent a critical security monitoring surface. With billions of daily authentication events, manual review is infeasible.

A robust classification and anomaly detection framework enables:

- Proactive identification of credential compromise attempts  
- Reduced false positives through behavioral modeling  
- Scalable security monitoring for large organizations  
