# Regression Analysis and Regularization Lab

**Course:** MSCS-634-M50 – Advanced Big Data and Data Mining  
**Student:** Elkana Munganga

## Purpose

This lab compares several regression methods on the scikit-learn Diabetes dataset (442 observations, 10 standardized health features, quantitative disease-progression target):

- **Simple Linear Regression** — BMI only, as a baseline
- **Multiple Linear Regression** — all 10 features
- **Polynomial Regression** — BMI with degrees 1, 2, 3, 5, and 10
- **Ridge Regression** — L2 regularization
- **Lasso Regression** — L1 regularization

All models use the same 80/20 split (353 train / 89 test, `random_state=42`) and are scored with MAE, MSE, RMSE, and R². The notebook is `lab4.ipynb`.

## Key Insights from the Regression Analysis

| Model | Test R² | RMSE |
| --- | ---: | ---: |
| Lasso (alpha = 0.1) | **0.4719** | **52.90** |
| Ridge (alpha = 0.1) | 0.4609 | 53.45 |
| Multiple Linear Regression | 0.4526 | 53.85 |
| Polynomial Regression (degree 10) | 0.2489 | 63.08 |
| Simple Linear Regression (BMI) | 0.2334 | 63.73 |

- BMI alone is a weak predictor. Simple linear regression explained only about 23% of test-set variation.
- Adding the other nine features roughly doubled R² (0.23 → 0.45) and cut RMSE from 63.7 to 53.9. Disease progression is a joint pattern, not a BMI-only story.
- Polynomial terms on BMI barely helped. Train R² rose only from 0.366 to 0.372 even at degree 10, and the best polynomial (degree 10, test R² 0.249) stayed near the simple linear baseline. Extra flexibility on one feature did not beat a linear model with all 10 features.
- Mild regularization beat unregularized multiple regression. Both Ridge and Lasso peaked at **alpha = 0.1**. Stronger penalties (alpha = 1 and 10) underfit: Lasso at alpha = 10 had test R² of −0.012.
- Lasso performed feature selection. At the best alpha it set **3 coefficients to zero** (age, s2, and s4). Ridge shrunk coefficients but kept all 10. BMI, s5, and blood pressure remained the largest effects.
- Best overall model: **Lasso with alpha = 0.1** (R² 0.4719, RMSE 52.90). Even the best model leaves more than half of the variation unexplained, so these results are statistical associations, not causal claims.

## Challenges and Decisions

- **Same split for every model.** I reused one train/test split so differences come from the model, not from a different random draw of patients.
- **Standardize features.** Diabetes columns are already scaled in scikit-learn, and I kept that scaling so Ridge and Lasso penalties treat features fairly. Unscaled inputs would let large-magnitude columns dominate the penalty.
- **BMI as the simple/polynomial feature.** BMI is a well-known clinical correlate, so it is a reasonable one-variable baseline. Polynomial models were also fit on BMI only, which made the comparison with simple linear regression direct.
- **How to pick the “best” polynomial.** I chose the degree with the highest **test** R² (degree 10), not the highest training R². That avoids rewarding a curve that only fits the training patients.
- **Alpha grid.** I tested 0.001, 0.01, 0.1, 1, and 10 to cover almost no penalty through strong shrinkage. Lasso used `max_iter=100000` so the larger alphas still converged.
- **Two views of coefficients.** Multiple regression produced large, unstable weights on the serum features (s1 and s2 in particular). Ridge and Lasso reduced those magnitudes, which is why a slightly higher test R² is more trustworthy than the unregularized coefficients.
- **Interpretation limit.** Even the winning Lasso model explains less than half of test-set variation. I treated the ranking as a comparison of predictive methods, not as a medical conclusion.
