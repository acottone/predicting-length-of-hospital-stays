# Predicting Hospital Length of Stay

Modeling hospital length of stay using Negative Binomial regression and regularized statistical learning on large-scale clinical data.

[![R](https://img.shields.io/badge/R-4.0%2B-blue)](https://www.r-project.org/)
[![GLM](https://img.shields.io/badge/Model-Negative%20Binomial-orange)](https://cran.r-project.org/package=MASS)
[![License](https://img.shields.io/badge/License-Academic-lightgrey)](LICENSE)

---

## TL;DR
- Modeled hospital length of stay as overdispersed count data
- Applied Negative Binomial GLMs with Lasso regularization
- Reduced predictors from 29 to 18 while preserving accuracy
- Achieved ~1.6-day average prediction error on 100,000 patients

---

## Project Overview

This project analyzes hospital patient data to identify key factors that influence length of stay (LOS) using advanced statistical modeling techniques. LOS is modeled as a count outcome, accounting for overdispersion commonly observed in healthcare utilization data.

Using the length of stay dataset from Microsoft
([Hospital Length of Stay Dataset](https://microsoft.github.io/r-server-hospital-length-of-stay/input_data.html))
with 100,000 patient records, I applied generalized linear models, regularization, and cross-validation to produce a parsimonious model suitable for clinical and operational decision-making.

---

## Dataset

**Source:** [Microsoft Hospital Length of Stay Dataset](https://microsoft.github.io/r-server-hospital-length-of-stay/)
- **File:** `LengthOfStay.csv`
- **Size:** 100,000 patient records
- **Variables:** 28 columns including demographics, comorbidities, laboratory results, and vital signs
- **Response Variable:** `lengthofstay` (discrete count variable, days)

---

## Methodology

### Analysis Pipeline

1. Data cleaning and exploratory analysis
2. Overdispersion diagnostics and model selection
3. Negative Binomial GLM fitting
4. Lasso regularization for variable selection
5. 10-fold cross-validation for model comparison
6. Performance evaluation and interpretability analysis

### Statistical Modeling Approach
1. Negative Binomial Regression
  - Chosen over Poisson and Quasi-Poisson due to overdispersion
  - Provides valid likelihood-based inference (AIC, confidence intervals)
  - Well-suited for healthcare utilization data
2. Lasso Regression (L1 Regularization)
  - Automated variable selection
  - Reduced model complexity
  - Improved generalization
3. Cross-Validation
  - 10-fold cross-validation
  - Compared link functions and model variants
  - Selected model with lowest RMSE

---

## Model Performance
- **RMSE:** 1.608 days
- **MAE:** 1.235 days
- **R^2:** 0.539
- **Final Predictors:** 18
- **AIC:** 301,363.9

The model predicts hospital LOS with ~1.6 days on average, providing actionable accuracy for operational planning.

---

## Key Results

### Significant Predictors of Length of Stay

#### Comorbidities
- Renal disease / dialysis
- Pneumonia
- Malnutrition
- Hematological conditions
- Substance dependence

#### Mental Health Indicators
- Major psychological disorder
- Depression
- Psychotherapy

#### Laboratory Values
- Neutrophil count
- Blood urea nitrogen (BUN)
- Hematocrit

#### Patient History
- Recent readmissions
- Multiple secondary diagnoses

---

## Model Refinement & Comparison

This project is an updated and improved version of an earlier LOS analysis.

### Structural Improvements
| Aspect | Original | Updated |
|--------|----------|---------|
| **Model Type** | Quasi-Poisson GLM | Negative Binomial GLM |
| **Model Complexity** | Polynomial + Interaction terms | Simpler linear terms only |
| **Variable Transformations** | Multiple (log, polynomial, centered) | Log transformations only |
| **Final Model Predictors** | 29 coefficients | 18 significant predictors |
| **Cross-Validation** | 10-fold CV | 10-fold CV with link function comparison |
| **RMSE** | 1.63 | 1.61 |
| **R^2** | 0.532 | 0.539 |

Similar predictive performance was achieved with 40% fewer parameters, improving interpretability and clinical usability.

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

## Installation & Requirements

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

### System Requirements
- R version 4.0.0 or higher
- RStudio (recommended)
- LaTeX distribution (for PDF output)

---

## Usage

### Running the Analysis
1. Clone this repository
2. Place `LengthOfStay.csv` in the project directory
3. Open `predictingLOS_updated.Rmd` in RStudio
4. Install required packages (see above)
5. Click "Knit" to generate PDF or HTML output

### Output Options
The R Markdown file supports multiple output formats:
- **PDF** (default) - Professional report with optimized figure sizing
- **HTML** - Interactive web-based report

---

## Key Insights

1. LOS is Highly Structured
  - A small subset of clinical factors explains most variation
  - Mental health and comorbidities play a major role
2. Parsimonious Models Perform Well
  - Removing unnecessary complexity improves stability
  - Easier to communicate results to clinicians
3. Practical Accuracy
  - ~1.6-day average error is sufficient for capacity planning
  - Slight overestimation is conservative and operationally safer

---

## Author

**Angelina Cottone**  
B.S. Statistics (Statistical Data Science), UC Davis 2025

---

## Acknowledgements
- Microsoft Hospital Length of Stay Dataset  
  ([Microsoft R Server Healthcare Demo](https://microsoft.github.io/r-server-hospital-length-of-stay/))

---
*Last Updated: January 2026*
