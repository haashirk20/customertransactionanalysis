# Customer Transaction Analysis and Fraud Detection

Exploratory analysis of 13.3 million card transactions in Python and Power BI, with two rule-based checks that flag possibly fraudulent activity.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-150458?logo=pandas&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?logo=powerbi&logoColor=black)
[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/haashirk20/customertransactionanalysis/blob/main/businessanalsytproject.ipynb)

## Demo

- [Open the notebook in Google Colab](https://colab.research.google.com/github/haashirk20/customertransactionanalysis/blob/main/businessanalsytproject.ipynb), or view [`businessanalsytproject.ipynb`](businessanalsytproject.ipynb) on GitHub with all charts rendered.
- The Power BI dashboard is in [`transactionBA.pbix`](transactionBA.pbix). Open it in Power BI Desktop.

## Overview

The project uses the Kaggle [Transactions Fraud Datasets](https://www.kaggle.com/datasets/computingvictor/transactions-fraud-datasets) (13,305,915 transactions). It joins each transaction to its merchant category code, cleans the amounts, timestamps and location fields, and looks at when, where and with whom customers spend. It then flags two kinds of suspicious activity: a card used in a different city within two hours of its last transaction, and a purchase far above that customer's usual spending.

## Key features

- Data cleaning in pandas: MCC descriptions joined from `mcc_codes.json`, duplicates dropped, `$` amounts parsed to floats, online transactions given `N/A` state and ZIP, and hour, weekday and month extracted from each timestamp.
- Charts for transaction volume by hour, top 10 customers by total spend, top 10 merchants by transaction count, top 10 merchant cities by amount spent, chip vs. non-chip transactions, and a per-customer spending scatter plot.
- An impossible-travel check that flags a customer's transaction when the merchant city changes within two hours of the previous one.
- A spending anomaly check that flags any transaction above the customer's mean plus three standard deviations.
- A Power BI report with a geographic heatmap of transactions across countries and US states, and a breakdown of total amount by revenue stream.
- The full cleaned dataset is exported to `df_merged.csv`.

## Tech stack

| Area | Tools |
|---|---|
| Analysis | Python, pandas, NumPy |
| Visualization | Matplotlib, Seaborn, Power BI |
| Environment | Google Colab |
| Data | Kaggle API |

## Technical highlights

- The travel check sorts transactions by customer and time, then uses `groupby().diff()` and `groupby().shift()` to compare each transaction with the same customer's previous one. Both checks are vectorised, with no Python loops over rows.
- The spending check computes each customer's mean and standard deviation once, merges them back onto the transactions, and compares every row in a single expression. Each customer is held to their own baseline, so a big spender isn't flagged for a normal purchase.

## Results

Charts and both fraud checks run on the first 20,000 rows of the cleaned data. The cleaning and the CSV export cover the full dataset. On that 20,000-row sample:

- The impossible-travel check flagged 2,612 transactions.
- The 3σ spending check flagged 203 transactions.

These are counts of flagged rows. The checks have not been scored against labelled fraud cases, so their precision is unknown.

## Getting started

The notebook downloads its data with the Kaggle API, so you need a Kaggle account and an API token (`kaggle.json`).

1. Open the notebook in [Google Colab](https://colab.research.google.com/github/haashirk20/customertransactionanalysis/blob/main/businessanalsytproject.ipynb).
2. Upload `kaggle.json` to the Colab session.
3. Run all cells. The first cell installs the Kaggle CLI, downloads `computingvictor/transactions-fraud-datasets`, and unzips `transactions_data.csv` and `mcc_codes.json`.

To run locally instead, install the dependencies and download the dataset yourself:

```sh
pip install pandas numpy matplotlib seaborn kaggle jupyter
kaggle datasets download -d computingvictor/transactions-fraud-datasets
unzip transactions-fraud-datasets.zip
jupyter notebook businessanalsytproject.ipynb   # skip the first cell, which sets up Kaggle for Colab
```

`businessanalsytproject.py` is Colab's export of the notebook. It contains `!` shell lines, so it won't run as a plain Python script.

## Contact

[LinkedIn](https://www.linkedin.com/in/mohammed-haashir-khan/) or `haashir.khan@mail.utoronto.ca`.
