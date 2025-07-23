# Motors RUL Prediction

This project predicts the Remaining Useful Life (RUL) of motors using sensor data. The goal is to help plan maintenance and avoid unexpected breakdowns.

## Project steps

1. **Load and analyze data**  
   We use a dataset with sensor readings from motors. First, we check the data, look for missing values, and understand the main features.

2. **Feature engineering**  
   We create new features using rolling windows, trends, and differences to better describe the motor's condition.

3. **Train/test split**  
   The data is split into training and test sets so we can check how well our models work.

4. **Model training**  
   We train several models: Linear Regression, XGBoost, and CatBoost. Each model tries to predict how many cycles the motor will last.

5. **Model evaluation**  
   We use different metrics to check model quality. The main metric is **S-score**. It is important because it gives a bigger penalty for underestimating the motor's life (which is risky in real life) than for overestimating.

## Why S-score metric is used

S-score is a special metric for RUL prediction. If the model predicts less life than the motor really has, it gets a big penalty. If it predicts more, the penalty is smaller. This helps make models safer for real use.

S-score is popular in competitions (like NASA C-MAPSS and Kaggle) because it reflects real risks in maintenance.

## How to use

- All code and explanations are in the notebook.
- You can run the notebook step by step to see how the models are built and tested.
- The S-score formula and code are also in the notebook.

## Requirements

- Python 3.8+
- pandas, numpy, scikit-learn, xgboost, catboost, matplotlib, seaborn, shap

## Data

The data file should be placed in `data/Data.csv`. It contains sensor readings and cycles for each motor.


## Model Comparison and Error Analysis

| Model              | MAE (all) | RMSE (all) | S-score (test) | S-score (CV)   | RUL ≤ 60 MAE | RUL ≤ 60 RMSE |
|--------------------|-----------|------------|----------------|---------------|--------------|--------------|
| Linear Regression  |   22      |   29       |   ~120,000     | ~19,000,000   |   10         |   13         |
| XGBoost            |  17.95    |  24.54     |   ~98,000      | ~18,800,000   |   9.08       |   14.02      |
| CatBoost           |  17.00    |  23.10     |   65,533       | 18,503,862    |   7.63       |   10.37      |

**Main conclusions:**
- CatBoost is the best model, showing the lowest errors and the best S-score.
- XGBoost also performs well, slightly behind CatBoost and much better than Linear Regression.
- All models sometimes overestimate the remaining useful life, which can be risky for maintenance planning.
- Most errors are small, but occasionally the models make large mistakes, especially for high RUL values.
- CatBoost is more reliable for short RUL values (≤ 60), which is important for safety.

**Extra analysis:**  
To better understand where the model makes mistakes, check the error distribution by RUL ranges (see the plot in the notebook).

> It is recommended to further improve the model or add a penalty for overestimation to make predictions safer for