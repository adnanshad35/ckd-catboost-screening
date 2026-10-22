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

**Source:** Rabie El Kharoua, *Chronic Kidney Disease Dataset*, Kaggle: https://www.kaggle.com/datasets/rabieelkharoua/chronic-kidney-disease-dataset-analysis. The CSV is included here only so the notebook runs as-is; see the Kaggle page for the dataset's license and terms.

The dataset is listed on Kaggle as **synthetic**. The results below describe model behavior on this dataset and should not be read as performance on real patient populations.

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

All models use the same split and the same validation-based threshold rule. Test-set results (n = 166):

| Model | Balanced Acc. | Sensitivity | Specificity | ROC-AUC | FN | FP |
|---|---:|---:|---:|---:|---:|---:|
| **Engineered CatBoost** | **0.7660** | 0.9605 | 0.5714 | 0.8210 | 6 | 6 |
| Random Forest | 0.7594 | 0.9474 | 0.5714 | 0.8313 | 8 | 6 |
| Logistic Regression | 0.7303 | 0.9605 | 0.5000 | 0.7744 | 6 | 7 |
| SVM | 0.6913 | 0.9539 | 0.4286 | 0.7914 | 7 | 8 |
| XGBoost | 0.6786 | 1.0000 | 0.3571 | 0.8341 | 0 | 9 |

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

Mean absolute SHAP values on the test set rank the following features highest:

1. Serum creatinine
2. Glomerular filtration rate (GFR)
3. Itching
4. Protein in urine
5. Blood urea nitrogen (BUN)
6. Systolic blood pressure
7. Muscle cramps

CatBoost's native feature importance gives the same top two (serum creatinine and GFR) and largely the same top seven.

These importance estimates describe the model’s behavior and should not be interpreted as evidence of clinical causality.

## Repository Structure

```text
ckd-catboost-screening/
├── Chronic_Kidney_Dsease_data.csv
├── ckd_catboost_screening.ipynb
├── requirements.txt
└── README.md
```

## Running the Notebook

1. Clone the repository:

```bash
git clone https://github.com/adnanshad35/ckd-catboost-screening.git
```

2. Install the pinned dependencies:

```bash
cd ckd-catboost-screening
pip install -r requirements.txt
```

3. Open `ckd_catboost_screening.ipynb` using Jupyter Notebook, JupyterLab, or Google Colab.

4. Run the notebook cells sequentially.

`requirements.txt` pins scikit-learn 1.6.1 and XGBoost 2.1.4, which reproduce the baseline numbers above exactly. Newer versions change the Random Forest and XGBoost baselines slightly; the CatBoost results are unchanged.

## Limitations

- The analysis uses a single dataset and has not undergone external or prospective validation.
- The test set contains only 14 non-CKD records; therefore, specificity and negative predictive value estimates are uncertain.
- The optimized threshold requires evaluation on an independent dataset.
- Model-derived feature importance does not establish causal relationships.
- This model is a research prototype and is not intended for clinical diagnosis or deployment.

## Manuscript Status

A conference manuscript based on this undergraduate thesis is currently being revised for future submission.

## Reproducibility Checks

- **End-to-end re-run:** the notebook was re-executed from a clean environment with `requirements.txt`; all reported numbers reproduce.
- **No imputation leakage:** the dataset has no missing values, so the median imputation step does not change any data.
- **Feature selection:** the 15 dropped features were chosen during exploratory analysis. As a check, the notebook retrains CatBoost on all 51 clinical features with the same split and threshold rule:

| Setting | Balanced Acc. | Sensitivity | Specificity | ROC-AUC |
|---|---:|---:|---:|---:|
| Selected features (38) | 0.7660 | 0.9605 | 0.5714 | 0.8210 |
| All features (51) | 0.7627 | 0.9539 | 0.5714 | 0.7942 |

  The results are nearly identical, so the feature selection does not drive the reported performance.
- **SHAP labels:** an earlier version of the SHAP table paired values with features in the wrong order; this is fixed, and the table and plot now use the test-set column order.
