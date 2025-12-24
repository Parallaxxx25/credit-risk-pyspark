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

# Corporate Credit Risk Rating System - Learner Guide

Welcome! This `README` is designed to help you understand the **`Credit_Risk_Project.ipynb`** notebook. This project builds a complete "Model-in-a-Box" for assessing the creditworthiness of companies.

## 1. The Goal
We want to answer a simple question: **"If we lend money to this company, will they pay us back?"**
To do this, we calculate a **Credit Rating** (AAA, AA, ... D) based on their financial data.

## 2. Notebook Walkthrough (Section by Section)

### Section 1: Configuration & Setup
Here we define the rules of the game.
- **RATING_MAPPING**: This is our "Lookup Table". It tells us that if a company has a 0.05% chance of default (`0.0005`), they are **AAA**. If they have a 25% chance (`0.25`), they are **C**.
- **Features**: We list the columns we care about (Revenue, Assets, etc.).

### Section 2: Synthetic Data Generation
**Why do we need this?** Real corporate financial data is expensive and private. We generate fake data to practice.
- **Key Logic**: We create 10,000 companies.
- **"Hidden" Risk**: We randomly make 15% of companies "risky" (high debt, low profit) so the model has something to find.
- **Target (`default`)**: We define "Default" as:
  1.  **Insolvency**: Liabilities > Assets (They owe more than they own).
  2.  **Liquidity Crisis**: Cannot pay short-term bills (*Current Ratio < 0.9*).
- **Spark**: We use `spark.createDataFrame` to load this into a distributed DataFrame provided by Databricks/PySpark.

### Section 3: Feature Engineering
Raw numbers (like "Total Assets: $10M") aren't enough. We need **Ratios** to compare big and small companies fairly.
- **Debt-to-Equity**: How much is borrowed vs. owned? (High is bad).
- **Current Ratio**: `Current Assets / Current Liabilities`. Can they pay bills due *now*? (Low is bad).
- **Interest Coverage**: `EBITDA / Interest Expense`. Can their profits cover loan interest?
- **VectorAssembler**: Spark ML requires all input numbers to be combined into a single column called `features`. This step does that packing.

### Section 4: Modeling
We use a **Random Forest Classifier**.
- **Why Random Forest?** It's robust and handles non-linear relationships well (e.g., a company is only risky if *both* debt is high *and* profit is low).
- **Training**: We teach the model on 80% of the data.
- **Testing**: We hide 20% of the data to test the model later.

### Section 5: Evaluation
How do we know it works?
- **AUC (Area Under ROC Curve)**: A score from 0.5 to 1.0.
  - **1.0** = Perfect model.
  - **0.5** = Guessing.
  - We aim for > 0.8.
- **Accuracy**: What % of companies did we classify correctly?

### Section 6: Risk Scoring & Calibration
This is the business translation layer.
- **Probability of Default (PD)**: The model outputs a probability (e.g., `0.02` or 2%).
- **Risk Score**: We convert this to a score (0-1000) for easier reading.
  - Formula: `(1 - PD) * 1000`.
  - Example: `1 - 0.02 = 0.98`. `0.98 * 1000 = 980`. Score is 980 (Very Good).
- **Rating**: We look up the PD in our `RATING_MAPPING` table to assign "A", "BBB", etc.

### Section 7: Migration Matrix
**Transition Risk**: If a company is "A" today, what will it be next year?
- We simulate next year by adding random "noise" to this year's probability.
- If a company moves from **A** -> **BBB**, that is a "downgrade".
- If a company moves from **A** -> **S** (Default), that is a crash.
- Banks needs this matrix to calculate capital reserves.

## Glossary
- **PySpark**: The Python tool for handling big data (millions of rows).
- **EBITDA**: Earnings Before Interest, Taxes, Depreciation, and Amortization (Roughly: "Operating Profit").
- **DataFrame**: A table of data (rows and columns).

