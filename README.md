# Predicting Length of Hospital Stays

## Overview
This project aims to predict the Length of Stay (LOS) in hospitals using patient data, such as demographics, comorbidities, and laboratory results. By identifying key factors that influence LOS, this project aims to enhance patient outcomes, improve hospital operations, and optimize resource allocation.

## Data Overview
- Data was accessed from the Microsoft R Server:
Microsoft. "Hospital Length of Stay Data." Microsoft R Server, n.d., https://microsoft.github.io/r-server-hospital-length-of-stay/input_data.html.
- **Size**: 100,000 rows x 28 columns.
- **Features**: Include demographics, comorbidities, laboratory results, and vital signs. Irrelevant variables (dates, IDs) were excluded, leaving 24 variables for analysis.

## Repository Structure
- *LengthOfStay.csv*: CSV file of the data used in project.
- *PredictingLengthofHospitalStayReport-part-1* & *PredictingLengthofHospitalStayReport-part-2*: Report detailing the project methods and results.
- *PredictingLengthofStay.Rmd*: RStudio file containing code.

## How to Use
- Open the .Rmd file in RStudio.

## Dependencies
- Required Libraries: psych, gridExtra, ggplot2, MASS, car, caret, lmtest, glmnet, corrplot, reshape2, GGally, viridis, dplyr

## Contact
- **Author**: Angelina Cottone
- **Email**: acottone0451@gmail.com
- **GitHub**: github.com/acottone
