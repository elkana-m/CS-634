# Data Visualization, Preprocessing, and Statistical Analysis Lab

**Course:** MSCS-634-M50 – Advanced Big Data and Data Mining

## Purpose

This lab practices the early stages of a data-mining workflow on a small product-sales dataset (`Date`, `Product`, `Units_Sold`, `Price`, `Customer_Rating`). The goals are to:

- Visualize the data and identify patterns or problems before modeling
- Clean and transform the data (missing values, outliers, reduction, scaling, discretization)
- Summarize the cleaned data with descriptive statistics and correlation

The notebook is `lab1.ipynb`.

## Key Insights from Visualizations and Statistics

**Visualizations**

- The bar chart of total units sold by product made Tablets look like the top seller. That ranking was driven by a single extreme value (100 units), not typical daily sales.
- The scatter plot of price vs. units sold showed most days clustered between 5 and 12 units. One point sat far above the rest, which flagged a likely outlier to handle in preprocessing.

**Statistical measures (after cleaning)**

- Units sold were fairly concentrated: min 5, max 12, mean 8.18, median 8, standard deviation 2.40, IQR 3.5.
- Mean and median were close, which is consistent with a more symmetric distribution once the outlier was removed.
- Price and customer rating were strongly positively correlated (about 0.87). Higher-priced products (especially laptops) tended to receive higher ratings.
- Units sold had almost no linear relationship with price (0.07) or rating (−0.04). Volume did not simply follow price or rating in this sample.

## Challenges and Decisions

- **Missing ratings.** Two `Customer_Rating` values were missing. I imputed them with the column mean so those rows could be kept. Dropping them would have been reasonable on a larger dataset, but here each row is a large share of the sample.
- **Outlier handling.** The IQR rule (upper bound 15.5) identified the Tablet sale of 100 units as an outlier. I removed it so it would not dominate totals, means, and later plots. Keeping it would have been appropriate only if it were a confirmed real event.
- **When to compute statistics.** I ran `info()`, `describe()`, central tendency, dispersion, and correlation on the cleaned 11-row dataset (`df_clean`), not on the 70% random sample. That keeps the summaries based on all remaining valid records.
- **Reduction vs. analysis.** Sampling 70% of rows and dropping `Date` was done to demonstrate data reduction. Those reduced/scaled tables were treated as a transformation exercise, not as the source for the statistical summary.
- **Scaling and bins.** Price was min-max scaled to [0, 1] so values with different magnitudes are easier to compare. Units sold were binned as Low (≤6), Medium (7–9), and High (≥10) using cut points that matched the cleaned range rather than equal-width bins across the original outlier-inflated scale.
- **Small sample.** With only 12 original rows, one missing value or one outlier has a large effect. Visual inspection and IQR were both needed before trusting product rankings or averages.
