# Multiple Linear Regression — Multi-Channel Marketing Analysis

## Environment Setup
```
pip install pandas numpy matplotlib seaborn statsmodels scipy
```

## Dataset
**marketing_sales_data.csv** — 572 records of marketing campaign spend and resulting Sales.

| Column | Type | Description |
|---|---|---|
| TV | categorical (Low/Medium/High) | TV ad spend tier |
| Radio | numeric | Radio ad spend ($K) |
| Social Media | numeric | Social media ad spend ($K) |
| Influencer | categorical (Nano/Micro/Macro/Mega) | Influencer partnership tier |
| Sales | numeric (target) | Resulting sales |

No missing values.

## Analysis Goals
Build a statistically robust Multiple Linear Regression model (via `statsmodels` OLS) to predict Sales, check for multicollinearity, validate model assumptions, and turn the results into a prioritized marketing budget recommendation.

## Approach
1. **EDA**: distributions, boxplots of Sales by TV tier and Influencer tier, pairplot of numeric features.
2. **Multicollinearity check**: correlation matrix + Variance Inflation Factor (VIF) on all predictors (TV ordinally encoded, Influencer one-hot encoded for this check only). All VIFs were well below 5 — no multicollinearity problem.
3. **Full model**: `Sales ~ C(TV) + Radio + Social Media + C(Influencer)` fit with OLS.
4. **Model refinement**: Social Media (p = 0.84) and Influencer tier (all p > 0.5) were not statistically significant and were dropped. The **reduced model** `Sales ~ C(TV) + Radio` was selected as final — Adjusted R² stayed virtually unchanged (0.9030 → 0.9035) while every remaining predictor is significant.
5. **Diagnostics**: residuals-vs-fitted plot, residual histogram, Q-Q plot, Shapiro-Wilk normality test, Breusch-Pagan homoscedasticity test.

## Final Model Performance

| Metric | Value |
|---|---|
| R-squared | 0.904 |
| Adjusted R-squared | 0.903 |
| F-statistic p-value | 1.8e-282 |

## Final Model Coefficients (`Sales ~ C(TV) + Radio`)

| Predictor | Coefficient | p-value | 95% CI |
|---|---|---|---|
| Intercept (TV=Low, Radio=0) | 64.23 | < .001 | [58.84, 69.62] |
| TV: Medium vs Low | 78.99 | < .001 | [72.48, 85.49] |
| TV: High vs Low | 154.30 | < .001 | [144.62, 163.98] |
| Radio | 2.97 | < .001 | [2.55, 3.38] |

**Interpretation**: Holding Radio spend constant, moving TV spend from Low to Medium is associated with a ~79-unit increase in Sales, and Low to High with a ~154-unit increase — TV is the dominant driver. Holding TV tier constant, each additional $1K of Radio spend is associated with a ~2.97-unit increase in Sales.

## Assumption Checks
- **Homoscedasticity**: Breusch-Pagan test p = 0.698 — fails to reject the null, supporting constant residual variance.
- **Normality**: Shapiro-Wilk test p < 0.001 on residuals — technically significant, but with n=572 even small deviations register; Q-Q plot and histogram show residuals are reasonably close to normal for inference purposes.
- **Linearity**: residuals-vs-fitted plot shows no strong systematic curve.

## Business Recommendations
1. **Prioritize TV ad spend** — by far the largest, most significant lever on Sales (High vs Low TV tier corresponds to the biggest coefficient in the model).
2. **Treat Radio as a reliable secondary channel** — smaller but statistically significant, near-linear positive return.
3. **De-prioritize Social Media and Influencer spend as currently measured** — neither showed a significant unique effect on Sales once TV and Radio were controlled for. This may reflect collinearity with TV/Radio campaigns rather than true ineffectiveness; recommend deeper investigation (e.g., engagement/awareness metrics, interaction terms) before cutting that budget outright.
4. **Validate with a controlled experiment** (e.g., geo-holdout test) shifting budget toward TV/Radio before committing to a full reallocation, since this is observational data and coefficients reflect association, not proven causation.

## Files
- `multiple_regression_analysis.ipynb` — full notebook, all cells executed, outputs visible.
- `marketing_sales_data.csv` — source dataset.
- `README.md` — this file.
- 
