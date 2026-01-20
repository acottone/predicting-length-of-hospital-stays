# Predicting Hospital Length of Stay

Statistical modeling of hospital length of stay using **Negative Binomial regression** and **regularized GLMs** on **100,000 patient records**.

![R](https://img.shields.io/badge/r-%23276DC3.svg?style=for-the-badge&logo=r&logoColor=white)\
![RStudio](https://img.shields.io/badge/RStudio-4285F4?style=for-the-badge&logo=rstudio&logoColor=white)

---

## TL;DR
- Modeled hospital length of stay as **overdispersed count data**
- Applied **Negative Binomial GLMs** with **Lasso regularization**
- Reduced predictors from **29 to 18** with minimal performance loss
- Achieved **~1.6-day average prediction error**
- Prioritized **interpretability and clinical usability**

---

## Project Overview

Hospital length of stay (LOS) is a critical driver of **hospital capacity planning, staffing, and cost control**. However, LOS data exhibit **strong overdispersion**, violating assumptions of standard Poisson models.

This project develops a **statistically principled and interpretable model** for LOS using:
- Likelihood-based **Negative Binomial regression**
- **Regularization** for variable selection
- **Cross-validation** for generalization assessment

The goal is not black-box prediction, but a **parsimonious, explainable model** suitable for healthcare decision-making.

---

## Key Findings

### Clinical Drivers of LOS
- Comorbidities (renal disease, pneumonia, malnutrition) dominate LOS risk
- Mental health indicators contribute meaningfully to prolonged stays
- Laboratory abnormalities reflect disease severity and resource needs

### Modeling Insights
- Negative Binomial regression significantly outperforms Poisson alternatives
- Regularization removes ~40% of predictors with negligible accuracy loss
- Simpler models improve interpretability without sacrificing utility

---

## Dataset

**Source:** [Microsoft Hospital Length of Stay Dataset](https://microsoft.github.io/r-server-hospital-length-of-stay/)
- **Patients:** 100,000
- **Predictors:** 28 clinical, demographic, and utilization variables
- **Response:** `lengthofstay` (days, discrete count)
- **Domain:** Inpatient hospital admissions

The dataset contains **no personally identifiable information** and is designed for healthcare analytics research.

---

## Methodology

This project follows a **structured statistical modeling pipeline**, emphasizing validity, interpretability, and reproducibility.

### 1. Data Preparation & Exploration
- Examined LOS distribution and variance structure
- Confirmed substantial overdispersion

### 2. Model Selection: Count Regression
- Evaluated Poisson, Quasi-Poisson, and Negative Binomial models
- Selected **Negative Binomial GLM** due to:
  - Overdispersion handling
  - Likelihood-based inference (AIC, confidence intervals)
  - Stability on large healthcare datasets

### 3. Regularization & Variable Selection
- Applied **Lasso (L1) regularization** via `glmnet`
- Reduced model complexity while preserving accuracy
- Improved generalization and clinical interpretability

### 4. Model Evaluation
- 10-fold cross-validation
- Compared link functions and model variants
- Evaluated RMSE, MAE, and pseudo-R²

### Results

| Metric | Value |
|--------|----------|
| **RMSE** | 1.608 days |
| **MAE** | 1.235 days |
| **R^2** | 0.539 |
| **Final Model Predictors** | 18 coefficients |
| **AIC** | 301,363.9 |

The model predicts LOS with **~1.6-day average error**, suitable for operational planning and risk stratification.

#### Key Predictors

**Comorbidities**
- Renal disease / dialysis
- Pneumonia
- Malnutrition
- Hematological conditions
- Substance dependence

**Mental Health**
- Major psychological disorder
- Depression
- Psychotherapy utilization

**Laboratory & History**
- Neutrophil count
- Blood urea nitrogen (BUN)
- Hematocrit
- Recent readmissions
- Multiple secondary diagnoses

#### Model Refinement & Comparison

This project improves upon an earlier LOS analysis by prioritizing statistical rigor and parsimony.

| Aspect | Original | Updated |
|--------|----------|---------|
| **Model Type** | Quasi-Poisson GLM | Negative Binomial GLM |
| **Model Complexity** | Polynomial + Interaction terms | Simpler linear terms only |
| **Variable Transformations** | Multiple (log, polynomial, centered) | Log transformations only |
| **Final Model Predictors** | 29 coefficients | 18 significant predictors |
| **Cross-Validation** | 10-fold CV | 10-fold CV with link function comparison |
| **RMSE** | 1.63 | 1.61 |
| **R^2** | 0.532 | 0.539 |

Comparable accuracy was achieved with **substantially reduced complexity**, improving interpretability.

---

## Reproducibility

### Environment
- R 4.0+
- RStudio (recommended)
- LaTeX (for PDF rendering)

### Run
1. Clone the repository
2. Place `LengthOfStay.csv` in the root directory
3. Open `predictingLOS_updated.Rmd`
4. Install required packages
5. Knit to PDF or HTML

### Required R Packages
```r
install.packages(c(
  "psych",        # Descriptive statistics
  "gridExtra",    # Multi-panel plots
  "ggplot2",      # Data visualization
  "MASS",         # Negative Binomial regression
  "car",          # VIF and diagnostics
  "glmnet",       # Lasso regression
  "caret",        # Cross-validation
  "lmtest",       # Durbin-Watson test
  "viridis",      # Color palettes
  "dplyr"         # Data manipulation
))
```

---

## Project Structure

```
predicting-length-of-hospital-stays/
│
├── LengthOfStay.CSV         # Dataset
├── predictingLOS_updated.Rmd  # Updated Rmd file with analysis
├── predictingLOS_updated.pdf  # Updated PDF report
├── original_project/        # Original project
│   ├── PredictingLengthofStay.Rmd  # Original Rmd file with analysis
│   └── PredictingLengthofStayReport.pdf  # Original PDF report
└── README.md                 # This file
```

---

## Technologies Used

- **R 4.0+**: Statistical computing
- **MASS**: Negative Binomial regression
- **glmnet**: Lasso regularization
- **caret**: Cross-validation and model evaluation
- **ggplot2**: Data visualization
- **dplyr**: Data manipulation

---

## Practical Implications
- Supports **bed capacity forecasting**
- Enables **risk stratification at admission**
- Favors **interpretability over black-box prediction**
- Conservative bias (slight overestimation) aligns with operational safety

---

## Author

**Angelina Cottone**  
B.S. Statistics (Statistical Data Science), UC Davis 2025

---
*Last Updated: January 2026*
