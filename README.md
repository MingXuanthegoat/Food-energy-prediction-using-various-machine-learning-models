# COMP4702 Assignment — Food energy prediction using various machine learning models

This project explores whether the energy content of foods in the Australian Food Composition Database (AFCD), Release 3, can be estimated from nutrient composition. It compares the conventional Atwater calculation with Random Forest and XGBoost regression models.

## Project contents

```text
.
├── code.ipynb
├── Datasets/
│   ├── AFCD Release 3 - Nutrient profiles.xlsx
│   └── AFCD_Release3_NutrientProfiles_All_Solids&liquids_per_100g.csv
├── Altered Data/                 # Saved model, feature, and analysis tables
└── Images/                       # Figures generated for the analysis
```

The notebook reads the CSV in `Datasets/`. The Excel workbook is included as the source dataset; the notebook does not convert it automatically.

## Getting started

Use Python 3 and install the packages used by the notebook:

```bash
python -m pip install jupyter pandas numpy matplotlib seaborn scikit-learn xgboost
```

From the project directory, start Jupyter and open `code.ipynb`:

```bash
jupyter notebook
```

Run the notebook cells from top to bottom. Its data paths are relative to the project root, so launch Jupyter from that directory. The Random Forest and XGBoost tuning steps use cross-validation and may take some time to run.

## Analysis overview

The notebook:

1. Loads the AFCD nutrient-profile CSV and uses `Energy with dietary fibre, equated (kJ)` as the prediction target.
2. Excludes identifier fields and the related `Energy, without dietary fibre, equated (kJ)` column from the model features.
3. Splits the data into 70% training and 30% test sets (`random_state=42`).
4. Checks nutrient data quality and evaluates a calculated Atwater estimate. The estimate uses 17 kJ/g for protein and available carbohydrate, 37 kJ/g for fat, and 29 kJ/g for alcohol.
5. Examines feature variance and correlation, removing highly correlated features at a Pearson correlation threshold of 0.95 for the model analysis.
6. Tunes Random Forest and XGBoost regressors using cross-validation, then compares their held-out test predictions with the Atwater baseline.
7. Exports result tables and visualizations.

## Test-set results

The following results are recorded in [`Altered Data/model_comparison_results.csv`](<Altered Data/model_comparison_results.csv>):

| Model              |     R² | RMSE (kJ) | MAE (kJ) |
| ------------------ | -----: | --------: | -------: |
| Atwater (baseline) | 0.9936 |     56.67 |    28.21 |
| Random Forest      | 0.9906 |     68.83 |    40.16 |
| XGBoost            | 0.9959 |     45.55 |    28.16 |

On this split, XGBoost has the highest R² and lowest RMSE; its MAE is similar to the Atwater baseline. Metrics are from the notebook's held-out test set and may depend on the dataset version and notebook execution environment.

## Outputs

The `Altered Data/` folder contains model-comparison results, feature importances, correlation analysis, cross-validation analysis, and variance analysis. The `Images/` folder contains the corresponding diagnostic and comparison figures, including:

- [`Images/model_comparison_bar.png`](Images/model_comparison_bar.png) — model R² comparison
- [`Images/predicted_vs_actual_all.png`](Images/predicted_vs_actual_all.png) — predicted versus actual energy
- [`Images/residuals_all.png`](Images/residuals_all.png) — model residual comparison
- [`Images/correlation_matrix_full.png`](Images/correlation_matrix_full.png) — nutrient-feature correlations

Some notebook cells also save plot files to the current working directory when rerun.
