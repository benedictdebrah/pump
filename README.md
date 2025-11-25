# Predictive Pump Failure Model  
*Machine Learning Pipeline for Early Failure Detection in Mud Pumps*

## Project Overview  
This repository contains the full project for developing a machine-learning model that predicts whether a mud pump (Mud Pump A or Mud Pump B) will fail within a specified time horizon (chosen: 7 days). The goal is to enable proactive maintenance planning, reduce unplanned downtime, and improve operational reliability.

## 📁 Repository Structure  
```

├─ PredictiveMaintenanceTestData.xlsx    ← Raw sensor & maintenance data
├─ notebook.ipynb                         ← Jupyter notebook with full workflow
├─ notebook.html                          ← HTML version of the notebook
├─ Pump Prediction model.docx             ← Detailed research report
├─ PredictiveMaintenanceReseachPaper.pdf  ← Guide
├─ pyproject.toml                         ← Project configuration
├─ uv.lock                                ← Dependency lock file
└─ README.md                              ← This file

````

## Problem Statement  
- Two pumps (Pump A and Pump B) are instrumented with sensors and maintenance history.  
- Using their operational data, the task is to predict whether a pump will **fail within the next 7 days**.  
- A binary classification model is built to warn maintenance teams in advance, allowing scheduling and parts provisioning.

## Approach Summary  
1. **Data Exploration & Cleaning**  
   - Addressed physically impossible values (e.g., negative current)  
   - Handled missing data, time‐stamp issues, and differentiated pump behaviours  

2. **Feature Engineering**  
   - Aggregated hourly sensor readings into daily averages for a 7-day horizon  
   - Calculated *min_days_left* across components and constructed the target label  
   - Removed leakage features (e.g., expected_life, days_left) to avoid trivial predictions  
   - Converted both pump datasets into a long format with a `pump_id` column  

3. **Model Training & Evaluation**  
   - Utilized a time‐series aware cross‐validation (TimeSeriesSplit) to simulate real deployment  
   - Trained a classifier (e.g., XGBoost) on combined pump data  
   - Assessed performance via confusion matrices, precision, recall, and F1‐score  

4. **Interpretation & Operational Implications**  
   - High recall ensures upcoming failures are detected early  
   - High precision ensures minimal false‐alarm maintenance  
   - Demonstrated robustness and generalizability across multiple folds  

## Key Results Summary  
- Early folds (e.g., fold 1) achieved ~0.89 accuracy as historical data was less stable.  
- Later folds reached 0.97 – 0.99 accuracy, with nearly perfect confusion matrices.  
- The model reliably differentiates between “fail within 7 days” and “healthy” states, supporting effective maintenance interventions.

## Why Use Both Pumps?  
Initial modelling using only Pump A or only Pump B failed to generalize due to class imbalance and temporal misalignment:  
- Pump A was heavily weighted toward imminent failure cases.  
- Pump B had many long-run healthy cases.  
Combining them created a richer and more balanced training set that improved model generalizability and operational relevance.


