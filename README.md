# End-to-End Corporate Credit Risk Rating System (PySpark)

This project implements a complete corporate credit risk rating system using PySpark, designed to be scalable and suitable for Databricks environments. It covers the entire pipeline from data generation to risk scoring and migration analysis.

## Overview

The system includes the following key components:

1.  **Synthetic Data Generation**: Generates realistic corporate financial data including assets, liabilities, revenue, and historical default indicators.
2.  **Feature Engineering**: Calculates key financial ratios (e.g., Debt-to-Equity, Current Ratio, ROA) and prepares features for modeling.
3.  **Machine Learning Modeling**: trains a Random Forest Classifier to predict the probability of default (PD).
4.  **Risk Scoring & Calibration**: Converts predicted probabilities into risk scores and assigns credit ratings (AAA to D) based on defined thresholds.
5.  **Migration Matrix Simulation**: Simulates how ratings might change over time using a stochastic process.

## Key Features

-   **Financial Ratio Calculation**: Computes standard financial metrics to assess company health.
-   **PySpark Machine Learning Pipeline**: Utilizes Spark MLlib for scalable feature transformation and model training.
-   **Rating System**: Maps predicted default probabilities to a standard credit rating scale (AAA, AA, A, BBB, ... D).
-   **Data Analysis**: Includes visualization of model performance (AUC, Accuracy) and rating migration.

## Configuration

The system uses a defined mapping for credit ratings based on Probability of Default (PD):

| Rating | PD Threshold |
| :--- | :--- |
| AAA | <= 0.05% |
| AA | <= 0.10% |
| A | <= 0.20% |
| BBB | <= 0.50% |
| BB | <= 1.00% |
| B | <= 3.00% |
| CCC | <= 8.00% |
| CC | <= 15.00% |
| C | <= 25.00% |
| D | > 25.00% |

## Prerequisites

-   **Python 3.8+**
-   **Apache Spark (PySpark)**
-   **Pandas**
-   **NumPy**

## Project Structure

-   `Credit_Risk_Project.ipynb`: The main notebook containing the full implementation.
-   `src/`: Source code directory (if applicable for modularized code).
-   `data/`: Directory for storing generated or input data.

## Usage

1.  Open the `Credit_Risk_Project.ipynb` notebook in a Jupyter environment or Databricks.
2.  Run the cells sequentially to:
    -   Generate synthetic data.
    -   Process features and train the model.
    -   Evaluate the model.
    -   View the assigned risk ratings and migration matrix.

## License

[Add License Information Here]
