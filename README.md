# Glass Type Classification — KNN & Random Forest

**Module:** Data Analytical Tools (L7 Diploma in Data Analytics) | CCT College Dublin  
**Author:** Rafael Da Rosa Barp  
**Date:** April 2026

---

## Overview

Data analytics pipeline applied to the Glass Identification Dataset (UCI Machine Learning Repository) to classify glass types based on chemical composition. Originally motivated by forensic criminology — where glass fragments at crime scenes serve as evidence — the analysis is applied here to support material classification for food packaging in an agricultural technology context.

The pipeline covers data preprocessing, descriptive and inferential statistical analysis, correlation analysis, and the development and comparison of two classification models: K-Nearest Neighbours (KNN) and Random Forest. An interactive Tableau dashboard complements the Python analysis.

**Dataset:** [Glass Identification — UCI ML Repository](https://doi.org/10.24432/C5WW2P) — 214 observations, 9 chemical composition features, 6 glass types.

---

## Key Results

| Metric | KNN (K=3) | Random Forest |
|---|---|---|
| Train/Test Accuracy | 77% | **84%** |
| CV Mean Accuracy (10-fold) | 67.1% | **73.7%** |
| CV Std Deviation | 0.087 | 0.129 |
| Macro Avg F1-Score | 0.67 | **0.82** |
| Weighted Avg F1-Score | 0.75 | **0.84** |

**Random Forest was selected as the preferred model** — outperforming KNN across all metrics, with perfect classification of glass type 7 (F1 = 1.00).

**Top predictors (Feature Importance):** Magnesium (16.2%), Aluminium (16.0%), Refractive Index (13.6%), Calcium (12.5%).

---

## Pipeline Structure

### 1. Data Preprocessing
- Removed non-informative `id` column
- No missing values found
- One duplicate record identified and removed
- Outliers retained — present across all glass types, deemed genuine chemical variation rather than noise

### 2. Normality Testing — Shapiro-Wilk
- All 9 features deviated significantly from normal distribution (p < 0.05)
- Result: non-parametric methods selected for inferential analysis

### 3. Homogeneity of Variance — Levene's Test
- Normality and homogeneity conditions failed for the majority of variables
- ANOVA not applicable → **Kruskal-Wallis** selected as non-parametric alternative

### 4. Inferential Analysis — Kruskal-Wallis Test
- Statistically significant differences found across all variables (p < 0.05)
- Chemical composition varies meaningfully between glass types — strong justification for predictive modelling

### 5. Correlation Analysis
- Pearson correlation heatmap revealed:
  - Magnesium (mg): strongest negative correlation with target (r = -0.74)
  - Aluminium (al): r = 0.60
  - Barium (ba): r = 0.57
  - Refractive index (ri) and calcium (ca): strong multicollinearity (r = 0.81)

### 6. Classification Models
- **KNN:** StandardScaler applied (distance-based classifier); K=3 selected via error rate analysis (K=1 to 30); 80/20 stratified split
- **Random Forest:** 100 trees; robust to class imbalance; interpretable via feature importance
- **Cross-validation:** 10-fold CV confirmed Random Forest superiority
