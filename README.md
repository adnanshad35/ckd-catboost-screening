# Engineered CatBoost for Imbalanced Chronic Kidney Disease Screening

This repository contains the implementation of an undergraduate thesis on machine-learning-based chronic kidney disease (CKD) screening. The study evaluates an engineered CatBoost pipeline for a highly imbalanced dataset while prioritizing sensitivity to reduce missed CKD cases.

The work was completed by a two-student research team under faculty supervision. Abu Adnan Shad led the model architecture, implementation, and results analysis.

## Research Objective

The study addresses two important challenges in CKD screening:

1. The substantial imbalance between CKD and non-CKD records.
2. The need to maintain high sensitivity while retaining meaningful performance for the minority non-CKD class.

CatBoost was evaluated against Logistic Regression, Random Forest, XGBoost, and Support Vector Machine (SVM) under a common train-validation-test framework.

## Dataset

- Total records: 1,659
- Modeling features: 38
- CKD records: 1,524 (91.9%)
- Non-CKD records: 135 (8.1%)
- Data split: 70% training, 20% validation, and 10% testing
- Split strategy: Stratified sampling with `random_state=42`

> The original dataset source, license, and citation must be added here before the repository is used for academic reproduction or redistribution.

## Methodology

The research workflow includes:

- Data inspection and preprocessing
- Feature selection
- Stratified training, validation, and test partitions
- Class-imbalance handling using balanced class weights
- Comparison with four baseline classifiers
- Decision-threshold optimization on the validation set
- Test-set evaluation using screening-relevant metrics
- Native CatBoost and SHAP-based feature interpretation

The classification threshold was selected under a minimum CKD sensitivity requirement of 0.92.

## Models Compared

The engineered CatBoost model was benchmarked against:

- Logistic Regression
- Random Forest
- XGBoost
- Support Vector Machine

## Test-Set Results

The selected CatBoost decision threshold was `0.425`. Its performance on the held-out test set (`n = 166`) was:

| Metric | Score |
|---|---:|
| Accuracy | 0.9277 |
| Balanced Accuracy | 0.7660 |
| Sensitivity (CKD Recall) | 0.9605 |
| Specificity | 0.5714 |
| Precision (PPV) | 0.9605 |
| Negative Predictive Value | 0.5714 |
| ROC-AUC | 0.8210 |
| PR-AUC | 0.9767 |
| F1-score | 0.9605 |

The test-set confusion matrix contained:

- True positives: 146
- True negatives: 8
- False positives: 6
- False negatives: 6

## Important Predictors

The leading predictors identified by CatBoost included:

- Serum creatinine
- Glomerular filtration rate (GFR)
- Protein in urine
- Blood urea nitrogen
- Age
- Electrolyte measurements
- HbA1c

These importance estimates describe the model’s behavior and should not be interpreted as evidence of clinical causality.

## Repository Structure

```text
ckd-catboost-screening/
├── Chronic_Kidney_Dsease_data.csv
├── ckd_catboost_screening.ipynb
└── README.md
```

## Running the Notebook

1. Clone the repository:

```bash
git clone https://github.com/adnanshad35/ckd-catboost-screening.git
```

2. Open the project directory:

```bash
cd ckd-catboost-screening
```

3. Open `ckd_catboost_screening.ipynb` using Jupyter Notebook, JupyterLab, or Google Colab.

4. Run the notebook cells sequentially.

The analysis uses Python with the following principal libraries:

- pandas
- NumPy
- Matplotlib
- Seaborn
- scikit-learn
- CatBoost
- XGBoost
- SHAP

A versioned dependency file will be added to support reproducible execution.

## Limitations

- The analysis uses a single dataset and has not undergone external or prospective validation.
- The test set contains only 14 non-CKD records; therefore, specificity and negative predictive value estimates are uncertain.
- The optimized threshold requires evaluation on an independent dataset.
- Model-derived feature importance does not establish causal relationships.
- This model is a research prototype and is not intended for clinical diagnosis or deployment.

## Manuscript Status

A conference manuscript based on this undergraduate thesis is currently being revised for future submission.

## Current Repository Status

The notebook and documentation are being reviewed for methodological clarity and reproducibility. Planned updates include leakage-safe preprocessing, verification of SHAP feature labels, reproducible feature-selection details, and a versioned dependency file.
