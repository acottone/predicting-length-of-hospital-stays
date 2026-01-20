# Predicting Hospital Length of Stay

Statistical analysis using Negative Binomial regression to predict hospital length of stay from patient characteristics, comorbidities, and laboratory values.

![R](https://img.shields.io/badge/r-%23276DC3.svg?style=for-the-badge&logo=r&logoColor=white)\
![RStudio](https://img.shields.io/badge/RStudio-4285F4?style=for-the-badge&logo=rstudio&logoColor=white)

---

## TL;DR
- **Predicted hospital length of stay** with 53.9% variance explained (R² = 0.539)
- **Simplified model** from 29 to 18 predictors while maintaining performance
- **Negative Binomial regression** outperforms Quasi-Poisson for overdispersed count data
- **RMSE of 1.608 days** - clinically useful prediction accuracy
- **Fixed data leakage** by excluding outcome-derived cluster variables
- **PDF-optimized visualizations** and professional document organization

---

## Project Overview

This project analyzes hospital patient data to identify key factors that influence length of stay (LOS) using advanced statistical modeling techniques. Understanding LOS predictors is crucial for improving patient outcomes, optimizing resource allocation, and enhancing operational efficiency in healthcare settings.

The analysis employs Negative Binomial regression to model count data with overdispersion (variance = 5.571, mean = 4.001), where traditional Poisson regression would be inadequate. Through systematic variable selection using Lasso regression and rigorous diagnostic testing, the final model identifies 18 significant predictors spanning comorbidities, mental health conditions, laboratory values, and demographic factors.

The project emphasizes:

- **Statistical rigor** - Comprehensive diagnostics (VIF, Durbin-Watson, Cook's Distance)
- **Model parsimony** - Simplified from 29 to 18 predictors without sacrificing performance
- **Clinical interpretability** - Linear terms preferred over complex polynomials and interactions
- **Reproducibility** - Well-documented R Markdown workflow with stratified cross-validation
- **Professional presentation** - PDF-optimized visualizations and organized supplementary materials

---

## From Coursework to Refined Analysis

This project originated as a statistical modeling assignment and has been significantly refined to address methodological issues and improve presentation quality.

### Original Implementation (`PredictingLengthofStay.Rmd`)

- Quasi-Poisson GLM with 29 coefficients
- Polynomial terms (quadratic + cubic) for 5 continuous variables
- 8 interaction terms between correlated predictors
- Cluster variable included in models (data leakage issue)
- Figures overflow PDF page margins
- Raw statistical outputs scattered throughout narrative

### Key Extensions/Improvements (`predictinglengthofstay_updated.Rmd`)

- **Negative Binomial GLM** - Proper likelihood-based inference with AIC model comparison
- **Simplified feature engineering** - Removed polynomials and interactions (18 predictors)
- **Fixed data leakage** - Cluster variable excluded from modeling
- **PDF-optimized visualizations** - All figures fit standard page dimensions
- **Professional organization** - Supplementary section for detailed outputs
- **Enhanced diagnostics** - VIF, Durbin-Watson, condition number, Cook's Distance
- **Better documentation** - Inline results, clear methodology explanations

---

## Key Findings

### Strongest Predictors of Longer Hospital Stays

**Mental Health & Substance Use:**
- Psychological disorders (major): +33.4% increase in LOS
- Depression: +29.9% increase
- Psychotherapy: +17.4% increase
- Substance dependence: +18.7% increase

**Chronic Conditions:**
- Dialysis/renal end-stage disease: +15.1% increase
- Asthma: +20.8% increase
- Pneumonia: +11.9% increase
- Anemia (hemo): +21.7% increase

**Laboratory Values:**
- Elevated blood urea nitrogen (log scale): +14.8% per unit increase
- Elevated neutrophils (log scale): +7.5% per unit increase

**Healthcare Utilization:**
- Prior readmissions (1+): +13.5% increase
- Multiple secondary diagnoses (3+): +11.2% increase

### Performance Summary

| Metric | Value | Clinical Interpretation |
|--------|-------|-------------------------|
| **RMSE** | 1.608 days | Average prediction error of ~1.6 days |
| **R²** | 0.539 | Explains 53.9% of variance in LOS |
| **MAE** | 1.235 days | Median prediction error of ~1.2 days |
| **AIC** | 301,363.9 | Best among 3 candidate models |
| **Theta** | 78,904.75 | Minimal overdispersion (nearly Poisson) |

**Model Validation:**
- Durbin-Watson: 2.004 (p = 0.734) ✓ No autocorrelation
- Max VIF: 1.531, Mean VIF: 1.178 ✓ No multicollinearity
- Condition Number: 117.49 ✓ Acceptable numerical stability

---

## Dataset

- **Source:** [Microsoft Hospital Length of Stay Dataset](https://microsoft.github.io/r-server-hospital-length-of-stay/)
- **Size:** 100,000 patient records
- **Variables:** 28 columns
  - **Demographics:** Age, gender
  - **Comorbidities:** 15 binary indicators (dialysis, asthma, pneumonia, etc.)
  - **Mental Health:** Depression, psychological disorders, psychotherapy
  - **Laboratory Values:** Hematocrit, neutrophils, sodium, blood urea nitrogen
  - **Vital Signs:** Pulse, respiration
  - **Healthcare Utilization:** Readmission count, secondary diagnoses
- **Response Variable:** `lengthofstay` (discrete count, 1-20 days)
- **Key Statistics:**
  - Mean LOS: 4.001 days
  - Variance: 5.571 (overdispersion present)
  - Median: 3 days
  - Range: 1-20 days

---

## Technical Implementation

### Algorithm: Negative Binomial Regression

The Negative Binomial model extends Poisson regression to handle overdispersion:

```
log(μᵢ) = β₀ + β₁X₁ᵢ + β₂X₂ᵢ + ... + βₚXₚᵢ

Var(Yᵢ) = μᵢ + (μᵢ²/θ)
```

Where:
- `μᵢ` = expected length of stay for patient i
- `θ` = dispersion parameter (larger θ → less overdispersion)
- `β` = coefficients (exponentiated to get incidence rate ratios)

**Final Model Equation (simplified):**
```
log(LOS) = 1.335 + 0.288×PsychDisorder + 0.261×Depression + 0.217×Hemo
           + 0.208×Asthma + 0.174×Psychotherapy + 0.187×SubstanceDepend
           + 0.151×Dialysis + 0.148×log(BUN+1) + 0.135×Readmit
           + 0.119×Pneumonia + ... [18 predictors total]
```

### Key Technical Decisions

#### 1. Negative Binomial over Quasi-Poisson

**Original:** Quasi-Poisson GLM (no likelihood, no AIC)
**Updated:** Negative Binomial GLM (proper likelihood-based inference)

**Rationale:**
- Enables AIC-based model comparison
- Provides accurate standard errors and confidence intervals
- Generates reliable prediction intervals for healthcare planning
- Theta parameter explicitly quantifies overdispersion

**Result:**
- AIC = 301,363.9 (lowest among 3 candidate models)
- Theta = 78,904.75 (minimal overdispersion, validates model choice)

#### 2. Simplification: Removing Polynomials and Interactions

**Original:** 29 coefficients (polynomials + interactions)
**Updated:** 18 predictors (linear + log transformations only)

**Rationale:**
- Polynomial and interaction terms added complexity without meaningful performance gain
- Original R² = 0.532 vs Updated R² = 0.539 (slight improvement with simpler model)
- Linear models are easier to interpret and communicate to clinicians
- Reduced risk of overfitting to training data

**Result:**
- 40% fewer parameters
- Better generalizability
- Clearer clinical interpretation

#### 3. Fixing Data Leakage: Excluding Cluster Variable

**Original:** K-means cluster variable included as predictor
**Updated:** Cluster variable excluded from modeling

**Rationale:**
- Clusters were created using LOS as input (circular reasoning)
- Including outcome-derived features as predictors inflates performance metrics
- Violates fundamental principle of predictive modeling

**Result:**
- Honest model performance estimates
- Clusters retained for exploratory analysis only

#### 4. Link Function Optimization

**Tested:** Identity, log, and sqrt link functions via 10-fold CV

**Result:**
- Identity link: RMSE = 1.608 (selected)
- Log link: RMSE = 1.612
- Sqrt link: RMSE = 1.615

**Rationale:** Identity link provides most accurate predictions while maintaining interpretability

---

## Methodology

**1. Data Acquisition & Preprocessing**
   - Load 100,000 patient records from `LengthOfStay.csv`
   - Check for missing values (none found)
   - Verify data types and distributions
   - Identify overdispersion (variance = 5.571, mean = 4.001)

**2. Feature Engineering**
   - Create binary variables: `rcount_binary` (0 vs 1+ readmissions), `gender_binary`
   - Recategorize secondary diagnoses: `secondarydx_recategorized` (0, 1-2, 3+)
   - Log-transform skewed variables: `log(neutrophils+1)`, `log(bloodureanitro+1)`
   - Bin LOS for visualization: `lengthofstay_binned` (1-2, 3-4, 5-6, 7+ days)

**3. Exploratory Data Analysis**
   - Univariate distributions (histograms, boxplots)
   - Principal Component Analysis (24 variables → 16 components explaining 80% variance)
   - K-means clustering (16 clusters, ANOVA confirms significant LOS differences)
   - Correlation matrix (identify multicollinearity candidates)
   - Scatterplots (assess linearity assumptions)

**4. Model Development**
   - **Full Model:** Negative Binomial GLM with 23 predictors (AIC = 301,370.8)
   - **Lasso Selection:** Cross-validated L1 regularization identifies 21 important predictors
   - **Lasso Model:** Refit Negative Binomial with Lasso-selected variables (AIC = 301,367.0)
   - **Final Model:** Remove non-significant predictors (p > 0.05) → 18 predictors (AIC = 301,363.9)

**5. Model Diagnostics**
   - Variance Inflation Factors (VIF < 2 for all predictors)
   - Durbin-Watson test (DW = 2.004, no autocorrelation)
   - Condition number (117.49, acceptable stability)
   - Cook's Distance (identify influential observations)
   - Residual plots (Q-Q plot, residuals vs fitted)

**6. Cross-Validation & Link Function Selection**
   - 10-fold cross-validation with stratified sampling
   - Compare identity, log, and sqrt link functions
   - Select identity link (lowest RMSE = 1.608)

**7. Model Evaluation**
   - Test set predictions (20% holdout, n = 20,000)
   - Performance metrics: RMSE, R², MAE
   - 95% prediction intervals
   - Actual vs predicted plots

---

## Results & Visualizations

### Model Comparison

| Model | Predictors | AIC | Theta | Status |
|-------|-----------|-----|-------|--------|
| Full Model | 23 | 301,370.8 | 78,918.06 | Baseline |
| Lasso-Selected | 21 | 301,367.0 | 78,917.35 | Improved |
| **Final Model** | **18** | **301,363.9** | **78,904.75** | **Best** |

### Original vs Updated Comparison

| Aspect | Original | Updated | Winner |
|--------|----------|---------|--------|
| **Model Type** | Quasi-Poisson | Negative Binomial | Updated |
| **Predictors** | 29 | 18 | Updated |
| **RMSE** | 1.63 days | 1.608 days | Updated |
| **R²** | 0.532 | 0.539 | Updated |
| **Interpretability** | Complex (polynomials + interactions) | Simple (linear + log) | Updated |
| **Data Leakage** | Yes (cluster variable) | No (excluded) | Updated |
| **PDF Output** | Figures overflow | Optimized sizing | Updated |

### Key Visualizations

**Exploratory Analysis:**
- Length of stay distribution (right-skewed, median = 3 days)
- PCA biplot (first 2 components explain 30% variance)
- K-means cluster heatmap (16 distinct patient groups)
- Correlation matrix (identifies multicollinearity)

**Model Diagnostics:**
- Residuals vs Fitted (checks homoscedasticity)
- Q-Q Plot (assesses normality of residuals)
- Cook's Distance (identifies influential observations)
- VIF bar chart (all values < 2)

**Predictions:**
- Actual vs Predicted scatter plot (R² = 0.539)
- Prediction intervals (95% coverage)
- Residual histogram (approximately normal)

---

## Reproducibility

### Environment

- **R Version:** 4.0 or higher
- **RStudio:** Recommended for R Markdown rendering
- **Operating System:** Cross-platform (Windows, macOS, Linux)
- **RAM:** 4GB minimum (8GB recommended for large dataset)

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/hospital-los-prediction.git
cd hospital-los-prediction

# Install required R packages
R -e "install.packages(c('psych', 'gridExtra', 'ggplot2', 'MASS', 'car', 'glmnet', 'caret', 'lmtest', 'corrplot', 'reshape2', 'GGally', 'viridis', 'dplyr'))"
```

**Required R Packages:**
- `psych` - Descriptive statistics and data summaries
- `gridExtra` - Multi-panel plot layouts
- `ggplot2` - Data visualization
- `MASS` - Negative Binomial regression (`glm.nb`)
- `car` - Variance Inflation Factors and diagnostics
- `glmnet` - Lasso regression for variable selection
- `caret` - Cross-validation and model training
- `lmtest` - Durbin-Watson test for autocorrelation
- `corrplot` - Correlation matrix visualization
- `reshape2` - Data reshaping for visualizations
- `GGally` - Extended ggplot2 functionality
- `viridis` - Colorblind-friendly color palettes
- `dplyr` - Data manipulation

### Usage

```bash
# Open the R Markdown file in RStudio
# File > Open File > predictinglengthofstay_updated.Rmd

# Knit to PDF (recommended)
# Click "Knit" button or run:
rmarkdown::render("predictinglengthofstay_updated.Rmd", output_format = "pdf_document")

# Knit to HTML (alternative)
rmarkdown::render("predictinglengthofstay_updated.Rmd", output_format = "html_document")
```

**Note:** Ensure `LengthOfStay.csv` is in the same directory as the R Markdown file.

### Outputs

- **PDF Report:** `predictinglengthofstay_updated.pdf` - Professional document with optimized figures
- **HTML Report:** `predictinglengthofstay_updated.html` - Interactive web version
- **Figures:** All visualizations embedded in the report
- **Model Objects:** Stored in R environment during execution

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

## Challenges & Limitations

### Technical Challenges Faced

**1. Overdispersion in Count Data**
- **Challenge:** Variance (5.571) significantly exceeds mean (4.001)
- **Solution:** Negative Binomial regression instead of Poisson
- **Outcome:** Theta = 78,904.75 (minimal overdispersion, model fits well)

**2. Multicollinearity Among Predictors**
- **Challenge:** Correlated comorbidities and lab values
- **Solution:** VIF analysis, removed highly correlated predictors
- **Outcome:** Max VIF = 1.531 (well below threshold of 5)

**3. Model Complexity vs Interpretability**
- **Challenge:** Original model had 29 coefficients (polynomials + interactions)
- **Solution:** Simplified to 18 linear predictors via Lasso and significance testing
- **Outcome:** Better interpretability with similar (slightly better) performance

**4. Data Leakage in Original Analysis**
- **Challenge:** Cluster variable derived from outcome included as predictor
- **Solution:** Excluded cluster variable from modeling
- **Outcome:** Honest performance estimates, clusters used for EDA only

### Current Limitations

- **Synthetic Data:** Dataset is simulated, not real patient records
- **Temporal Factors:** No time-series information (admission date, seasonal trends)
- **Hospital Characteristics:** No facility-level variables (size, location, teaching status)
- **Treatment Information:** No data on interventions, procedures, or medications
- **Generalizability:** Model trained on single dataset, may not transfer to other hospitals
- **Prediction Accuracy:** RMSE of 1.6 days may be too large for some clinical applications

### Scope Boundaries

- **Focus:** Statistical modeling and prediction, not causal inference
- **Outcome:** Length of stay only (not readmission, mortality, or cost)
- **Methods:** Regression-based approaches (no machine learning algorithms tested)
- **Validation:** Single train/test split (no external validation dataset)

---

## Technologies Used

### Programming & Analysis
- **R** (4.0+) - Statistical computing and graphics
- **RStudio** - Integrated development environment
- **R Markdown** - Reproducible research and dynamic reporting
- **LaTeX** - PDF document generation

### Statistical Methods
- **Negative Binomial Regression** - Overdispersed count data modeling
- **Lasso Regression (L1 Regularization)** - Automated variable selection
- **Principal Component Analysis** - Dimensionality reduction
- **K-means Clustering** - Unsupervised patient segmentation
- **10-Fold Cross-Validation** - Model performance assessment

### R Packages
- **MASS** - `glm.nb()` for Negative Binomial GLM
- **glmnet** - `cv.glmnet()` for Lasso regression
- **caret** - `train()` for cross-validation and link function comparison
- **car** - `vif()` for multicollinearity diagnostics
- **lmtest** - `dwtest()` for autocorrelation testing
- **ggplot2** - Data visualization with grammar of graphics
- **viridis** - Perceptually uniform color palettes

---

## Author

**Angelina Cottone**  
B.S. Statistics (Statistical Data Science), UC Davis 2025

---

## References

1. 

---
*Last Updated: January 2026*
