# Predicting Hospital Length of Stay

## Project Overview

This project analyzes hospital patient data to identify key factors that influence length of stay (LOS) using advanced statistical modeling techniques. Understanding LOS predictors is crucial for improving patient outcomes, optimizing resource allocation, and enhancing operational efficiency in healthcare settings.

**Research Question:** *What are the key factors that influence the length of hospital stays for patients?*

## Dataset

- **Source:** `LengthOfStay.csv`
- **Size:** 100,000 patient records
- **Variables:** 28 columns including demographics, comorbidities, laboratory results, and vital signs
- **Response Variable:** `lengthofstay` (discrete count variable, days)

## Key Features

### Statistical Methods
- **Negative Binomial Regression** - Accounts for overdispersion in count data
- **Lasso Regression** - Automated variable selection with L1 regularization
- **Principal Component Analysis (PCA)** - Dimensionality reduction and pattern identification
- **K-means Clustering** - Patient segmentation (16 clusters)
- **10-Fold Cross-Validation** - Robust model performance assessment

### Model Performance
- **RMSE:** 1.608 days (average prediction error)
- **R-squared:** 0.539 (explains 53.9% of variance)
- **MAE:** 1.235 days (average absolute error)
- **Final Model:** 18 significant predictors with lowest AIC (301,363.9)

## Project Files

### Main Analysis Files
- **`predictingLOS_updated.Rmd`** - Current, improved analysis (recommended)
- **`PredictingLengthofStay.Rmd`** - Original analysis (archived)
- **`LengthOfStay.csv`** - Dataset

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

## Project Structure

### Analysis Workflow
1. **Data Acquisition & Processing** - Import, cleaning, feature engineering
2. **Exploratory Data Analysis** - Distributions, correlations, PCA, clustering
3. **Modeling** - Full model, Lasso selection, final model refinement
4. **Model Diagnostics** - VIF, Durbin-Watson, residual analysis
5. **Prediction Analysis** - Test set evaluation, prediction intervals
6. **Supplementary Tables** - Detailed statistical outputs
7. **R Appendix** - Complete code reference

## Key Findings

### Significant Predictors of Length of Stay

**Comorbidities (Positive Association):**
- Dialysis/renal end stage disease
- Asthma
- Iron deficiency
- Pneumonia
- Substance dependence
- Malnutrition
- Hematological conditions

**Mental Health Indicators (Strong Positive Effects):**
- Major psychological disorder
- Depression
- Psychotherapy

**Laboratory Values (Higher = Longer Stay):**
- Hematocrit levels
- Neutrophil counts
- Blood urea nitrogen (BUN)

**Patient Characteristics:**
- Recent readmissions (within 180 days)
- Secondary diagnoses
- Demographic factors

## Significant Improvements from Original Version

The updated analysis (`predictingLOS.Rmd`) represents a major enhancement over the original project (`PredictingLengthofStay.Rmd`) with the following improvements:

### 1. **Simplified and More Robust Statistical Methodology**

| Aspect | Original | Updated |
|--------|----------|---------|
| **Model Type** | Quasi-Poisson GLM | Negative Binomial GLM |
| **Model Complexity** | Polynomial + Interaction terms | Simpler linear terms only |
| **Variable Transformations** | Multiple (log, polynomial, centered) | Log transformations only |
| **Final Model Predictors** | 29 coefficients | 18 significant predictors |
| **Cross-Validation** | 10-fold CV | 10-fold CV with link function comparison |

**Why Negative Binomial over Quasi-Poisson?**
- Provides proper likelihood-based inference (enables AIC comparison)
- More accurate standard errors and confidence intervals
- Reliable prediction intervals for healthcare planning
- Better statistical properties for overdispersed count data
- Theta parameter explicitly models overdispersion

### 2. **Streamlined Model Complexity**

**Original Approach:**
- Polynomial terms (quadratic and cubic) for 5 continuous variables
- Centered variables to reduce multicollinearity
- 8 interaction terms between correlated predictors
- Respiration squared term
- **Result:** 29 coefficients, complex interpretation

**Updated Approach:**
- Simple log transformations for skewed variables only
- No polynomial or interaction terms
- Three-stage refinement: Full (23) → Lasso (21) → Final (18)
- **Result:** 18 significant predictors, clearer interpretation

**Benefits of Simplification:**
- Easier to interpret and communicate to clinicians
- Reduced risk of overfitting
- Better generalizability to new data
- Maintains similar predictive performance (R² = 0.539)
- More stable coefficient estimates

**Result:** All diagnostic tests passed - model meets statistical assumptions

### 3. **Improved Exploratory Data Analysis**

**Original EDA:**
- Basic distributions and correlations
- PCA and K-means clustering (16 clusters)
- Cluster variable **included in models** (potential data leakage)

**Updated EDA:**
- Same PCA and clustering analysis
- **Cluster variable excluded from models** (prevents data leakage)
- More detailed interpretation of cluster patterns
- Enhanced correlation matrix visualization
- Better organized presentation of findings

**Critical Fix:** Clusters are now used only for exploratory insights, not as predictors, avoiding circular reasoning where the outcome influences the predictor

### 4. **PDF-Optimized Visualizations**

**Original Visualizations:**
- `fig.width = 10` (too wide for PDF pages)
- Figures overflow page margins
- Inconsistent sizing across plots

**Updated Visualizations:**
- ✅ **PDF-Optimized Sizing** - `fig.width = 7, fig.height = 4.5` (fits standard pages)
- ✅ **Custom Dimensions** - Individual sizing for multi-panel plots
- ✅ **Consistent Color Schemes** - Viridis palettes throughout
- ✅ **Professional Layout** - 1-inch margins, proper spacing
- ✅ **Multi-Panel Efficiency** - `grid.arrange()` for side-by-side comparisons

**Result:** All figures now fit properly on PDF pages without overflow or distortion

### 5. **Professional Document Organization**

**Original Structure:**
- Raw outputs scattered throughout main text
- Long model summaries interrupt narrative flow
- Difficult to distinguish key findings from details

**Updated Structure:**
- **Cleaner Main Text** - Focuses on interpretation and key findings
- **New Section 6: Supplementary Tables** - Detailed outputs organized separately
- **Inline Results** - Key metrics embedded in narrative (e.g., AIC = 301,363.9)
- **Better Flow** - Logical progression without interruptions
- **Professional Formatting** - Follows academic paper conventions

**Outputs Moved to Supplementary Section:**
- Data summary and structure
- Cluster statistics and ANOVA results
- Full model and Lasso model detailed summaries
- Complete VIF tables
- Confidence intervals
- Prediction intervals sample

**Result:** Main document is 30-40% shorter and much more readable

### 6. **Enhanced Cross-Validation**

**Original Cross-Validation:**
- 10-fold CV for model comparison
- Tested models with/without polynomial and interaction terms
- RMSE and R² reported

**Updated Cross-Validation:**
- **10-Fold Cross-Validation** - Same robust approach
- **Link Function Comparison** - Tested identity, log, and sqrt links
- **Optimal Selection** - Identity link chosen (RMSE = 1.608, lowest)
- **Comprehensive Metrics** - RMSE, R², MAE all reported
- **Better Documentation** - Clear explanation of CV methodology

**Clinical Interpretation:** Model predicts LOS within ~1.6 days on average, suitable for healthcare planning

### 7. **Cleaner Feature Engineering**

**Original Feature Engineering:**
- Binary variables: `rcount_binary`, `gender_binary`, `secondarydx_recategorized`
- Log transformations: `hematocrit`, `neutrophils`, `bloodureanitro`
- Centered variables: 5 continuous variables centered
- Polynomial terms: Quadratic and cubic for 5 variables
- Interaction terms: 8 specific interactions

**Updated Feature Engineering:**
- **Same binary variables** (kept what worked)
- **Simplified transformations:** Only log transforms for `neutrophils` and `bloodureanitro`
- **Removed:** `hematocrit` transformation (not significant)
- **Removed:** All polynomial terms (unnecessary complexity)
- **Removed:** All interaction terms (minimal improvement, added complexity)
- **Removed:** Centered variables (not needed without polynomials)

**Result:** Simpler, more interpretable features with similar predictive power

### 8. **Improved Prediction Analysis and Interpretation**

**Original Prediction Analysis:**
- Test set predictions
- Basic performance metrics
- Residual plots

**Updated Prediction Analysis:**
- **Same test set approach** (80/20 split)
- **95% Prediction Intervals** - Quantifies uncertainty
- **Comprehensive Metrics** - MAE, RMSE, R² clearly reported
- **Multiple Diagnostic Plots** - Residuals, Q-Q plots, Cook's Distance
- **Better Interpretation** - Clinical context for all metrics
- **Actual vs Predicted Plots** - Visual assessment of model performance

**Test Set Performance:**
- R² = 0.539 (explains 53.9% of variance)
- RMSE = 1.608 days
- Model slightly overestimates LOS (conservative, good for healthcare planning)

### 9. **Technical and Code Quality Improvements**

**Code Quality Enhancements:**
- **Warning Suppression** - Clean output without "iteration limit reached" warnings
- **Better Documentation** - More detailed comments explaining methodology
- **Cleaner Code** - Removed redundant data splits and unnecessary complexity
- **Reproducibility** - Consistent use of `set.seed(2024)`
- **Inline Results** - Use of R Markdown inline code for dynamic reporting

**Why Suppress "Iteration Limit" Warnings?**
- Theta value is very large (78,904.75) indicating minimal overdispersion
- Algorithm struggles to precisely estimate such large theta values
- Model still converges successfully - warning is cosmetic, not problematic
- All diagnostics confirm model validity

**Removed Redundancies:**
- Original had duplicate data splitting code
- Updated has single, clean data split

## Model Comparison: Original vs Updated

### Original Project Final Model
- **Model Type:** Quasi-Poisson GLM
- **Predictors:** 29 coefficients
- **Complexity:** Polynomial (quadratic + cubic) + 8 interaction terms
- **RMSE:** 1.63 days
- **R²:** 0.5318
- **Dispersion:** 0.615

### Updated Project Final Model
- **Model Type:** Negative Binomial GLM
- **Predictors:** 18 significant predictors
- **Complexity:** Simple linear terms with log transformations
- **RMSE:** 1.608 days
- **R²:** 0.539
- **AIC:** 301,363.9
- **Theta:** 78,904.75

### Key Differences
| Aspect | Original | Updated | Improvement |
|--------|----------|---------|-------------|
| **Interpretability** | Complex (polynomials + interactions) | Simple (linear + log) | Much easier to explain |
| **Predictors** | 29 coefficients | 18 predictors | More parsimonious |
| **Statistical Framework** | Quasi-Poisson (no AIC) | Negative Binomial (AIC-based) | Better model comparison |
| **Performance** | R² = 0.532 | R² = 0.539 | Slightly better |
| **Overfitting Risk** | Higher (complex model) | Lower (simpler model) | Better generalization |
| **Clinical Usability** | Difficult to apply | Easy to apply | More practical |

**Conclusion:** The updated model achieves similar (slightly better) predictive performance with 40% fewer parameters and much simpler structure, making it more interpretable, generalizable, and clinically useful.

## Reproducibility

### Setting Up the Environment
```r
# Set working directory
setwd("path/to/your/project")

# Load all required packages
library(psych)
library(gridExtra)
library(ggplot2)
library(MASS)
library(car)
library(glmnet)
library(caret)
library(lmtest)
library(viridis)
library(dplyr)

# Set random seed for reproducibility
set.seed(2024)
```

### Data Requirements
The analysis expects `LengthOfStay.csv` with the following structure:
- Patient records (rows)
- 28 variables (columns) including:
  - Demographics (age, gender)
  - Comorbidities (binary indicators)
  - Laboratory values (continuous)
  - Vital signs (continuous)
  - Length of stay (response variable)

### Expected Runtime
- **Full Analysis:** ~5-10 minutes (depending on system)
- **PDF Generation:** ~2-3 minutes additional
- **Total:** ~10-15 minutes

## Clinical Implications

### For Healthcare Providers
1. **Risk Stratification** - Identify high-risk patients early
2. **Resource Planning** - Better bed management and staffing
3. **Care Coordination** - Focus on comorbidity and mental health management
4. **Quality Improvement** - Target interventions for modifiable risk factors

### For Hospital Administrators
1. **Capacity Planning** - Predict bed occupancy more accurately
2. **Cost Management** - Anticipate resource needs
3. **Performance Metrics** - Benchmark against predicted LOS
4. **Decision Support** - Data-driven operational decisions

## Limitations

1. **Model Assumptions** - Assumes independence of observations (validated via DW test)
2. **Generalizability** - Results specific to this dataset; may not generalize to all hospitals
3. **Temporal Factors** - Does not account for seasonal variations or time trends
4. **Missing Variables** - Some clinical factors (e.g., surgical procedures) not included
5. **Prediction Accuracy** - RMSE of 1.6 days means some predictions may be off by 2+ days

## License

This project is available for educational and research purposes. Please cite appropriately if used in academic work.

## Author

**Angelina Cottone**

## Acknowledgments

- Dataset source: `LengthOfStay.csv`
- Statistical methods: Negative Binomial regression, Lasso regularization
- Visualization: ggplot2 and viridis color palettes
- R Community: For excellent packages and documentation
---
**Last Updated:** January 2026
