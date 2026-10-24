# The 5 Research Questions

## 1. Identifying high commodity-price months

**Research question:** Which months from 2010 to 2026 had a total BCPI value above its historical median, and what were the corresponding CEER values during those months?
**What you would do:** Clean the date columns, merge the two datasets, remove rows with missing values in the selected variables, calculate the median BCPI, and filter for months above it.

Pandas skills: to_datetime(), merge(), dropna(), Boolean filtering with .loc[], and median().
Why it fits: This is primarily data cleaning and filtering, with a small descriptive analysis. You do not need regression or advanced statistics.

## 2. Commodity-price increases vs. decreases

**Research question:** Does the average monthly change in the CEER differ between months when the BCPI increases and months when it decreases?
**What you would do:** Calculate the month-over-month percentage change in each index, classify months according to whether the BCPI rose or fell, and compare the average CEER change between the two groups.

Pandas skills: pct_change(), creating conditional categories, groupby(), mean(), and a bar chart.
Why it fits: It builds directly on basic data cleaning and filtering but introduces derived variables and grouped summaries.

## 3. Commodity prices and exchange-rate movements

**Research question:** How strongly are monthly changes in the overall, energy, and non-energy BCPI associated with changes in the nominal and real CEER?
**What you would do:** Calculate percentage changes, produce correlation matrices and scatterplots, and compare the strength and direction of the relationships across commodity groups and exchange-rate measures.

Pandas skills: pct_change(), corr(), grouping and reshaping data, and visualization with Matplotlib or Seaborn.
Why it fits: This introduces statistical relationship analysis and compares several economic variables instead of examining one filtered subset.

## 4. Did the relationship change over time?

**Research question:** Did the relationship between monthly BCPI changes and nominal CEER changes differ before, during, and after the COVID-19 economic disruption?
**What you would do:** Divide the dataset into three periods: 2010–2019, 2020–2022, and 2023–the latest available month. Calculate and compare the correlations for each period, then visualize the results.

Pandas skills: Date-based filtering, assigning time-period categories, groupby(), correlation calculations, and plotting.
Why it fits: You must combine time-based data manipulation with comparative statistical analysis and interpret whether the relationships differ across periods.

## 5. Can commodity prices help predict the exchange rate?

**Research question:** Do past BCPI movements improve predictions of next month's CEER change beyond what can be predicted using previous CEER movements alone?
**What you would do:** Create lagged features for the overall, energy, and non-energy BCPI, along with past CEER changes. Build a baseline model using only historical CEER changes, then compare it with a model that also includes BCPI information. Evaluate both on later months that were excluded from training.

andas skills: Lagged columns, time-series feature engineering, missing-value handling, chronological train/test splits, and model evaluation using MAE or RMSE.
Why it fits: This combines data preparation, predictive modelling, careful evaluation, and the need to avoid data leakage by never training on future observations.
Important: This question tests predictive value, not whether commodity prices cause exchange-rate movements.

## 6. Cross-dataset economic exploration

**Research question:** How are Canadian labour-market conditions, oil prices, and inflation connected with commodity prices and Canadian effective exchange rates?

**What you would do:** Add relevant monthly labour-market, oil-price, and inflation data from reliable official sources. Align the additional datasets with the BCPI and CEER data using their shared monthly date index, restrict all datasets to the same analytical period, and compare trends and relationships across the variables. The analysis could examine correlations, changes during important economic periods, and whether labour-market conditions, oil prices, or inflation help explain movements in commodity prices and the Canadian dollar.

Potential variables may include:

- Employment and unemployment measures
- The unemployment rate
- Labour-force participation
- Crude oil prices
- Consumer price inflation
- Core inflation measures
- Changes in the overall BCPI, energy BCPI, and CEER

Pandas and analysis skills: Date alignment, merging multiple datasets, handling missing observations, resampling, percentage changes, correlation analysis, grouped comparisons, visualization, and interpretation of cross-dataset relationships.

Why it fits: This extends the project beyond the relationship between commodity prices and exchange rates by incorporating broader economic indicators. It can help assess whether observed relationships are also associated with labour-market conditions, oil prices, or inflation. This analysis should be interpreted as exploratory because associations between these variables do not establish causation.