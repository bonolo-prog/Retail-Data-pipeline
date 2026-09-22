# Retail-Data-pipeline

Building a retail data pipeline to extract, transform, aggregate and load e-commerce data

## Background

By the end of 2022, Walmart's e-commerce business had grown to $80 billion in sales which is 13% of the company's total revenue. One key factor influencing that revenue is public holidays, such as the Super Bowl, Labor Day, Thanksgiving, and Christmas. This project builds a data pipeline to ingest, clean, and aggregate Walmart's sales data, in order to analyze how weekly sales trends shift around these holiday periods.

## Pipeline Overview

- **Extract** - Pulls grocery sales data from a PostgreSQL database and merges it with supplementary data (holidays, economic indicators, store details) from a Parquet file.
- **Transform** - Cleans the merged data by imputing missing values, extracting the month from each sale's date, filtering out weeks with sales below $10,000, and dropping unused columns.
- **Aggregate** - Groups the cleaned data by month and calculates average weekly sales, rounded to two decimal places.
- **Load** - Saves both the cleaned dataset and the aggregated results as CSV files.
- **Validate** - Confirms that both output files were successfully created before considering the pipeline complete.

## Key Decisions & Challenges

- **Handling missing values in `Weekly_Sales`:** I initially considered imputing missing values with the column mean, but reasoned that doing so before filtering for sales over $10,000 could introduce fabricated data into that threshold check and skew the later monthly aggregation. I left those values unfilled instead, since a pandas comparison against `NaN` naturally excludes them from the filter.

- **`.dt` accessor error:** I initially wrote `raw_data.dt.month` to extract the month from the `Date` column, assuming `.dt` would work directly on the whole DataFrame. This raised an error, which led me to learn that the `.dt` accessor only works on a single Series (column) of datetime values, not an entire DataFrame. I corrected it to `raw_data['Date'].dt.month`, referencing the specific column first.
