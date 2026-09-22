# CKD Screening Using an Engineered CatBoost Model

This repository contains the implementation of **“Engineered CatBoost Model for Imbalanced CKD Screening.”** The project evaluates a CatBoost-based machine-learning pipeline for screening chronic kidney disease (CKD) from clinical and lifestyle variables.

The study focuses on two practical challenges: substantial class imbalance and the need to maintain high sensitivity when identifying individuals with CKD. CatBoost is compared with Logistic Regression, Random Forest, XGBoost, and Support Vector Machine (SVM) using a common evaluation framework.

## Project Overview

The workflow includes:

- Data cleaning and preprocessing
- Clinically informed feature removal
- Stratified training, validation, and test splits
- Class-imbalance handling
- Decision-threshold optimization
- Comparison with four baseline models
- Evaluation using screening-relevant performance measures
- Native CatBoost and SHAP-based feature analysis
- Generation of publication-ready figures

## Repository Structure

```text
ckd-catboost-screening/
├── Chronic_Kidney_Dsease_data.csv
├── ckd_catboost_screening.ipynb
└── README.md
