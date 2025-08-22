# Bank Marketing Campaign Success Prediction

## Project Overview

This project aims to build a predictive model to identify bank customers who are most likely to subscribe to a term deposit. By accurately predicting potential subscribers, Nexent Bank can optimize its marketing campaigns, reduce operational costs, and improve the conversion rate of term deposits.

The motivation stems from the low deposit ratio observed in previous campaigns, which impacts the bank's operational liquidity (MRR and Basel III/CRR compliance). Focusing marketing efforts on high-potential customers is crucial for increasing the deposit base efficiently.

## Problem Statement

How can Nexent Bank identify and predict customers with a high probability of opening a term deposit to increase conversion rates and optimize marketing costs?

## Goals

1.  Develop a classification model capable of predicting whether a customer will subscribe to a term deposit.
2.  Identify the key factors and variables that most influence a customer's decision to make a deposit, providing insights for more effective marketing strategies.

## Data

The dataset used in this project is sourced from: [https://www.kaggle.com/datasets/volodymyrgavrysh/bank-marketing-campaigns-dataset](https://www.kaggle.com/datasets/volodymyrgavrysh/bank-marketing-campaigns-dataset)

It contains information about bank marketing campaigns, including customer demographics, social and economic context attributes, and campaign-related details. The target variable is `y`, indicating whether the customer subscribed to a term deposit ('yes') or not ('no').

**Key Data Characteristics:**

*   Highly imbalanced dataset (majority class: 'no').
*   Includes a mix of numerical and categorical features.
*   Each row represents a customer contacted during a marketing campaign.

## Methodology

The project follows a standard data science pipeline:

1.  **Business and Data Understanding:** Initial analysis of the business problem, cost/benefit trade-offs (highlighting the importance of minimizing False Negatives), and a detailed look at the raw data features.
2.  **Data Cleaning & EDA:** Handling missing values (specifically 'unknown' and '999' in `pdays`), removing duplicates, and visualizing data distributions and correlations (Pearson for numerical, Cramer's V for categorical) to gain insights into feature relationships and their potential impact on the target variable.
3.  **Feature Engineering:** Creating new, more informative features or transforming existing ones:
    *   Binning `age` into `age_group`.
    *   Grouping similar categories in `marital`, `education`, and `job`.
    *   Creating binary features for `housing`, `loan`, and `pdays`.
    *   Calculating `total_contacts`.
    *   Using PCA on macroeconomic factors (`emp.var.rate`, `cons.price.idx`, `cons.conf.idx`) to create composite indices (`macro_index_1`, `macro_index_2`).
4.  **Model Selection:** Evaluating multiple classification models (Decision Tree, Random Forest, Extra Trees, XGBoost, LightGBM) using cross-validation. The evaluation focused on metrics relevant to the business problem, particularly Recall (to minimize missed opportunities/False Negatives) and F1-Score (for overall balance). XGBoost was selected due to its high Recall score.
5.  **Hyperparameter Tuning:** Optimizing the selected XGBoost model's hyperparameters using `GridSearchCV` to improve performance metrics, with a focus on balancing Precision and Recall using the F1-Score as the refit metric and adjusting `scale_pos_weight` to handle class imbalance.
6.  **Model Evaluation:** Detailed evaluation of the best XGBoost model on the test set using Classification Report, Confusion Matrix, ROC Curve (AUC), and Precision-Recall Curve.
7.  **Feature Importance & Interpretability:** Analyzing the importance of features using XGBoost's F-Score and SHAP values to understand which factors drive the model's predictions and how they influence the probability of a positive outcome.
8.  **Prediction & Recommendations:** Applying the final model to predict the deposit probability for all customers and providing strategic recommendations for campaign implementation and future improvements based on the analysis.

## Key Findings and Results

*   The final XGBoost model achieved a Recall of **0.61** and Precision of **0.39** on the test set (at a threshold of 0.4). This indicates the model's effectiveness in identifying potential depositors while accepting a manageable level of false positives.
*   Key factors influencing deposit decisions include: **Euribor 3-month rate (euribor3m), total number of contacts, customer age group, and macroeconomic conditions (macro\_index\_1, macro\_index\_2).** Specific months (March, December, September, October) and previous campaign outcomes ('success') also showed high success rates.
*   A simulation based on the confusion matrix demonstrates that using the model for targeted marketing can lead to significant cost savings and increased deposit conversion compared to a non-targeted approach.
