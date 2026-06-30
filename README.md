# Multiple Linear Regression - Multi-Channel Marketing Analysis

## Overview
Analyzing how TV budget level, Radio spend, Social Media spend, and Influencer type
affect Sales using a Multiple Linear Regression (OLS) model built with statsmodels.
The analysis includes multicollinearity diagnostics (correlation + VIF), full OLS
assumption checking, and business recommendations tied directly to the model's
statistically significant coefficients.

## Setup
pip install pandas numpy seaborn statsmodels matplotlib scipy

## Files
- `multiple_regression_analysis.ipynb` - Main analysis notebook (all cells executed, outputs visible)
- `marketing_sales_data.csv` - Dataset (TV, Radio, Social Media, Influencer, Sales)

## Analysis Steps
1. Load and explore the dataset
2. Encode categorical variables (TV ordinal, Influencer one-hot)
3. Exploratory data analysis (distributions, scatter plots, boxplots)
4. Multicollinearity check (correlation matrix + VIF)
5. Build Multiple Linear Regression model (OLS via statsmodels)
6. Print final regression equation
7. Check OLS assumptions (Residuals vs Fitted, Q-Q plot, Scale-Location, residual histogram, Durbin-Watson)
8. Interpret coefficients and statistical significance
9. Deliver business recommendations based on the model

## Key Findings
- The model explains about 90% of the variation in Sales (R-squared = 0.904).
- **TV budget level** is the strongest, most statistically significant driver of Sales
  (coefficient = 77.45, p < 0.0001).
- **Radio spend** is also statistically significant (coefficient = 2.96, p < 0.0001).
- **Social Media spend** and **Influencer type** are not statistically significant
  predictors of Sales once TV and Radio are accounted for (p > 0.37 in all cases).
- TV_encoded (VIF=6.5) and Radio (VIF=13.1) show moderate-to-high multicollinearity,
  noted and discussed in the notebook as a caveat to coefficient precision.

## Recommendation
Prioritize TV advertising budget, followed by Radio spend, for the strongest impact
on Sales. Social Media spend and influencer tier selection should be guided by
factors other than expected Sales lift, since this model finds no reliable effect
from either.
