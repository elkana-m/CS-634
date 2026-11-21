# MSCS-634 Project Deliverable 1

## Data Collection, Cleaning, and Exploration

**Student** Minh Le, Sebastian Capkovi, Shreyas More, Elkana Munganga
**Course:** MSCS-634 – Advanced Big Data and Data Mining  
**Dataset:** German Credit Data

## Dataset Overview

This project analyzes the German Credit dataset, which contains 1,000 records and nine meaningful attributes describing credit applicants and their loans. The dataset includes demographic information, account categories, loan amounts, durations, and purposes.

Dataset source: https://www.kaggle.com/datasets/varunchawla30/german-credit-data

## Data Cleaning

We loaded the dataset using Pandas and inspected its structure, data types, and descriptive statistics.

The cleaning process included:

1. Removing the unnecessary index column.
2. Identifying missing values in the savings and checking account columns.
3. Replacing missing categorical values with "Unknown."
4. Checking for and removing exact duplicate records.
5. Examining categorical attributes for inconsistencies.
6. Checking numerical features for potentially invalid values.
7. Saving the cleaned dataset for future analysis.

## Exploratory Data Analysis

We used Matplotlib and Seaborn to explore the dataset through histograms, count plots, boxplots, scatter plots, pair plots, and a correlation heatmap.

Our analysis focused on customer age, credit amount, loan duration, housing status, account categories, and loan purposes.

## Key Insights

Our exploratory analysis helped identify differences in borrowing characteristics and potential outliers in numerical features.

We also examined relationships between credit amounts, loan durations, and other attributes to identify useful features for future modeling.

The results provide a foundation for regression, classification, clustering, and association rule mining in subsequent deliverables.

## Challenges and Solutions

**Missing values:** Savings and checking account information contained missing values. We replaced them with "Unknown" rather than removing observations or assuming account ownership.

**Potential outliers:** Some numerical observations were identified as potential outliers. We retained them because they may represent legitimate financial records.

**Categorical features:** Several attributes contain categorical values that will require encoding before applying machine learning algorithms.

**Classification target:** The dataset does not include an explicit credit-risk outcome variable. We will need to define a suitable classification objective for future work.

## Tools and Libraries

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## Project Files

- `German_Credit_EDA.ipynb` — Data cleaning, exploration, visualizations, and analysis.
- `german_credit_data.csv` — Original dataset.
- `german_credit_cleaned.csv` — Cleaned dataset.
- `README.md` — Project documentation.