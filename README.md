# US Mortgage Lending Data: Missing Data and Influence Analysis

How much does a mortgage pricing benchmark built from public data depend on how missing values and extreme loans are handled? This project audits 515,746 Home Mortgage Disclosure Act (HMDA) records for Georgia in 2025 and measures how a benchmark of interest rates changes with each choice, for 108,384 home purchase loans.

## Notebooks

| Notebook | What it does |
|---|---|
| [01_data_audit_and_preparation](01_data_audit_and_preparation.ipynb) | Downloads a dated snapshot, audits the table (29 findings), records a decision for each problem without overwriting any raw value, and builds the analysis dataset with a data dictionary |
| [02_missing_incomes_and_extreme_loans](02_missing_incomes_and_extreme_loans.ipynb) | **What the Data Leaves Out.** Diagnoses the gaps, tests four ways of handling them in a simulation with a known truth, compares the benchmarks they give on the real loans, and measures which loans drive the results |

## Main findings

- Dropping incomplete loans gives the same benchmark as multiple imputation for site-built homes, within 2.9% on every coefficient, but understates the coefficients for manufactured homes by 27.5% to 66.5%.
- Mixing the two markets in one model makes the handling method matter: the income coefficient differs by 18.7% between complete cases and multiple imputation.
- In a simulation of 1,200 samples, filling gaps with the mean shrank the coefficients by 22% to 69%, while multiple imputation stayed within 2.0% of the truth when gaps followed observed information.
- One lender's incomes, reported in the wrong unit, cut the income coefficient by about 61%; only the audit catches this kind of error.
- No single loan steers the benchmark, but different treatments of extreme rates move the income coefficient between 13% below and 6% above its least-squares value.

## Methods

Missing-data patterns, Little's MCAR test, missingness models, complete-case analysis, mean and regression imputation, multiple imputation by chained equations with Rubin's rules, Monte Carlo simulation (bias, RMSE, coverage), delta-adjustment sensitivity analysis, classic and robust (MCD) Mahalanobis distances, leverage, studentized residuals, Cook's distance, DFBETAS and Huber regression.

## Data

Home Mortgage Disclosure Act data published by the FFIEC and the CFPB through the HMDA Data Browser, Georgia, 2025, snapshot of 2026-11-09. HMDA public data are a work of the US federal government. The demographic fields of applicants are not used, and the project is not an assessment of fair lending.

## Repository structure

    01_data_audit_and_preparation.ipynb
    02_missing_incomes_and_extreme_loans.ipynb
    data/
      raw/hmda_GA_2025_2026-11-09.csv.gz                  snapshot as downloaded (33 of 99 columns)
      processed/hmda_GA_2025_analysis_2026-11-09.parquet  analysis dataset (49 columns)

## Reproducing the results

Both notebooks run in Google Colab with Python. Notebook 1 uses pandas, requests and pyarrow, and loads the stored snapshot instead of downloading it again. Notebook 2 reads the processed file from this repository and also uses NumPy, matplotlib, SciPy, statsmodels and scikit-learn. All random steps use a fixed seed, so a full run gives the same numbers; it takes about half an hour, mostly for the simulation and the multiple imputations.

## Author

Emely Lephilibert
