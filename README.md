# Proactive Fraud Detection in Financial Transactions 🕵️‍♂️

This project is an exploration into building a robust machine learning model to detect fraudulent financial transactions. I was curious to see if I could use data science to identify the patterns of fraudulent behavior in a large-scale dataset and build a system that could flag suspicious activity in real-time.

---

## 🎯 Project Goal

The main goal of this project was to develop a highly accurate classification model to distinguish between legitimate and fraudulent transactions. Beyond just prediction, I wanted to understand the key factors and behaviors that are strong indicators of fraud.

---

## 📊 The Data

The dataset used is a synthetic financial dataset from Kaggle, which simulates mobile money transactions. It's a challenging dataset with over **6.3 million records**, making it a great playground for practicing with large-scale data.

* **Source:** [Synthetic Financial Datasets For Fraud Detection on Kaggle](https://www.kaggle.com/datasets/ealaxi/paysim1)

* **Key Features:** `step`, `type`, `amount`, `oldbalanceOrg`, `newbalanceOrig`, `isFraud`.

A major challenge with this data is the severe **class imbalance**, with fraudulent transactions making up only about 0.1% of the total records.

---

## 🛠️ My Approach

I followed a standard data science workflow to tackle this problem:

1. **Data Cleaning & EDA:** The first step was to get a feel for the data. I checked for missing values and duplicates, and then dove into exploratory data analysis. Visualizing the transaction types confirmed that fraud exclusively occurs in `TRANSFER` and `CASH_OUT` transactions.

2. **Feature Engineering:** To give the model more to work with, I created some new features based on logical assumptions about fraud:

   * `origBalanceDiscrepancy`: This feature flags transactions that don't follow standard accounting logic (i.e., `new_balance + amount != old_balance`), which turned out to be a huge indicator of fraud.

   * `isOrigBalanceZero`: This flags suspicious "mule account" activity, where an account starts and ends with a zero balance.

3. **Modeling:** To ensure a robust solution, I decided to build and compare two different types of models:

   * **Random Forest Classifier:** An excellent tree-based model that is great with tabular data and provides clear feature importances.

   * **Artificial Neural Network (ANN):** A deep learning model to see if it could capture any more complex, non-linear patterns.

   To handle the severe class imbalance, I used the **`RandomUnderSampler`** technique, which creates a balanced dataset for the models to train on.

---

## 📈 Key Findings & Performance

Both models performed exceptionally well, but the Random Forest was slightly better and more interpretable.

* **High Recall:** The models successfully identified over **99% of all fraudulent transactions** on the unseen test data. This is the most critical metric, as the goal is to miss as little fraud as possible.

* **Excellent ROC-AUC Score:** Both models achieved a score of over **0.99**, indicating a near-perfect ability to distinguish between classes.

The Random Forest model revealed the most important factors for predicting fraud:

| Feature | Importance | Why it makes sense | 
 | ----- | ----- | ----- | 
| `origBalanceDiscrepancy` | \~0.34 | Represents a clear mathematical error in the transaction. | 
| `oldbalanceOrg` | \~0.16 | Fraud often targets accounts with specific balances. | 
| `newbalanceOrig` | \~0.14 | Fraudulent transactions often empty an account to zero. | 
| `type_TRANSFER` | \~0.05 | A primary method used to move stolen funds. | 

---
