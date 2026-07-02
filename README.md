# Predicting County-Level Housing Prices Using Zillow Data and Apache Spark

A Spark-based regression pipeline that forecasts next-month county-level housing prices using Zillow's Housing Value Index (ZHVI) dataset.

## Objective

Housing prices are a key indicator for investment decisions and market trends. This project analyzes county-level housing price trends across five U.S. states and builds predictive models to forecast short-term future prices using recent price history.

## Dataset

- **Source:** Zillow Housing Value Index (ZHVI) — middle-tier (33rd–67th percentile) single-family homes and condos, seasonally adjusted
- **States:** New Jersey, New York, Pennsylvania, California, Texas
- **Time range:** 2010–2025
- **Size after preprocessing:** 78,633 county-month observations

## Pipeline

1. **Preprocessing (Apache Spark):** filtered to 5 states and 2010+, reshaped from wide to long time-series format using Spark's `stack()` function, dropped nulls
2. **Feature engineering:** built `lag_1`, `lag_3`, `lag_12` month price features per county using Spark window functions; target = next month's price via `lead()`
3. **Train/test split:** time-based split — train on 2010–2022 (57,009 rows, 78.3%), test on 2023–2025 (15,761 rows, 21.7%)
4. **Models:** Linear Regression, Gradient Boosted Trees, and Random Forest, all built with Spark ML Pipelines

## Results

| Model | RMSE | MAE |
|---|---|---|
| **Linear Regression** | **$3,120.14** | **$2,082.42** |
| Gradient Boosted Trees | $73,152.80 | $29,117.50 |
| Random Forest | $71,093.77 | $23,697.80 |

Linear Regression outperformed both tree-based models by over 20x, likely because housing prices change gradually over time, favoring a simple, smooth relationship between past and future prices over the decision-split logic of tree ensembles.

### Per-state error breakdown (Linear Regression)

| State | RMSE | MAE |
|---|---|---|
| PA | $1,720 | $1,340 |
| NY | $2,511 | $1,655 |
| NJ | $2,647 | $2,004 |
| TX | $2,676 | $2,032 |
| CA | $5,741 | $3,637 |

Error scales with average home price by state — CA has the highest average home value ($429K) and highest error, while PA has the lowest average value ($166K) and lowest error.

## Tech Stack

Apache Spark, PySpark, Spark ML, Spark SQL Window Functions

## Team

Anthi Lyra, Jinsu Mathew — DSCI 632, Drexel University

## Future Work

Incorporate additional predictors such as interest rates and broader economic indicators to improve prediction accuracy.
