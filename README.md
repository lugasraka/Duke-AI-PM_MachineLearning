# Combined Cycle Power Plant Power Output Prediction

## Overview
This project predicts the full-load electrical power output (PE, in MW) of a base-load operated Combined Cycle Power Plant (CCPP) using machine learning. The work explores multiple modeling approaches, evaluates their performance with robust metrics, and interprets results to derive actionable business insights.

- Product goal: Provide reliable and interpretable predictions of PE to support operations planning, capacity management, and efficiency optimization.
- Problem statement: Given ambient and process conditions — Ambient Temperature (AT), Atmospheric Pressure (AP), Relative Humidity (RH), and Vacuum/Exhaust Steam Pressure (V) — predict PE accurately and efficiently for unseen conditions.

## Data
- Source: Pınar Tüfekci (2014), International Journal of Electrical Power & Energy Systems, Volume 60, Pages 126-140.
- Features: AT (°C), AP (mbar), RH (%), V (cm Hg)
- Target: PE (MW)
- File: CCPP_data.csv

## Methods
We trained and compared four supervised regression models:
- Model 1: Simple Linear Regression using `AT` only
- Model 2: Multiple Linear Regression using `AT, V, AP, RH`
- Model 3: Decision Tree Regressor using `AT, V, AP, RH`
- Model 4: Random Forest Regressor using `AT, V, AP, RH`

### Workflow summary
- Exploratory analysis: Descriptive stats, correlation matrix, scatter/pair plots
- Data split: 70% train / 30% test with `random_state=100`
- Evaluation: MAE, MAPE, RMSE, R², 5-fold cross-validation score, training time
- Visualization: Actual vs. predicted scatter plots with RMSE annotations
- Reporting: Metrics consolidated to an Excel file `model-metrics.xlsx`

## Performance Interpretation
- R²: Measures explained variance; closer to 1 indicates stronger predictive alignment.
- RMSE: Penalizes larger errors, in MW; lower is better.
- MAE: Average absolute error in MW; interpretable magnitude of typical error.
- MAPE: Percentage error; useful for relative accuracy across operating ranges.
- Training time: Computational efficiency; relevant for model retraining and deployment SLAs.
- Cross-validation: 5-fold mean score indicates generalization beyond a single train/test split.

## Results Analysis
Key observations drawn from the notebook’s analysis:
- AT exhibits strong negative correlation with PE; as ambient temperature rises, expected output decreases, consistent with thermodynamic efficiency effects.
- Linear models (particularly multiple linear regression) capture global trends well and offer high interpretability with low training time.
- Tree-based models (Decision Tree, Random Forest) can capture nonlinear interactions and often reduce RMSE at the cost of higher complexity and potentially longer training.
- Random Forest generally provides the best bias-variance tradeoff, reflected in strong R² and RMSE, and stable cross-validation performance.
- Scatter plots of actual vs. predicted values show tighter clustering around the parity line for multi-feature models and Random Forest, indicating better calibration.

## Business Insights
- Operational planning: Use predicted PE to schedule generation commitments, anticipate shortfalls during high AT conditions, and plan reserve margins.
- Efficiency optimization: Monitor conditions (AT, V, AP, RH) to adjust operating setpoints aiming to mitigate performance degradation (e.g., inlet air cooling during hot periods).
- Maintenance & risk: Deviations between actual and predicted PE may flag equipment or sensor issues; track residuals to trigger inspections.
- Capacity forecasting: Incorporate forecasts of weather variables (AT, RH, AP) into PE predictions for day-ahead and week-ahead scheduling.
- Portfolio strategy: Random Forest’s robust performance suggests a production model can balance accuracy and interpretability (with feature importance) for stakeholder communication.

## How to Run
1. Install dependencies in your Python environment (e.g., venv or conda):

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl
```

2. Open the notebook `CC_powerPrediction-Coursera-AIPM.ipynb` and run cells sequentially.
3. Ensure `CCPP_data.csv` resides in the same folder as the notebook.
4. Outputs:
   - Visual EDA and model comparison plots
   - Metrics summary displayed as a DataFrame
   - Exported metrics to `model-metrics.xlsx`

## Repository Structure
- CCPP_data.csv — dataset file
- CC_powerPrediction-Coursera-AIPM.ipynb — full analysis and modeling
- Machine Learning for Combined Cycle Power Plant  Power Output Prediction.gslides — presentation materials
- README.md — project overview, methods, and insights

## Next Steps
- Hyperparameter tuning (e.g., Random Forest: `n_estimators`, `max_depth`, `min_samples_split`) with cross-validation.
- Feature engineering (e.g., interaction terms, transformations) to capture nonlinearities.
- Incorporate exogenous forecasts (weather inputs) for real-time and day-ahead predictions.
- Model monitoring with residual analysis and drift detection in production.

## License
This repository is for educational and demonstration purposes. Add a specific license if you plan to distribute or reuse the code and data.
