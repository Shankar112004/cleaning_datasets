# 🧹 Data Cleaning & EDA Portfolio | Python · Pandas · Seaborn

![Python](https://img.shields.io/badge/Python-3.x-blue) ![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-informational) ![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-teal) ![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-orange)

**Messy data in, analysis-ready data out.** Two real-world datasets, cleaned step by step with Pandas, then explored with EDA and visualizations. Every notebook documents what was wrong with the data, how I fixed it, and what the clean data says.

> **Shankar Dixit** | MCA student | Aspiring Data Analyst | Python · SQL · Power BI
> 📧 dixitshankar84@gmail.com · [LinkedIn](https://linkedin.com/in/shankar-dixit) · [GitHub](https://github.com/Shankar112004)

---

## 📌 At a glance

| | Notebook 1: Real Estate | Notebook 2: Retail Sales |
|---|---|---|
| File | `cleaning 1.ipynb` | `cleaning_2.ipynb` |
| Data | Surat property listings | Retail store transactions |
| Raw size | 4,525 rows × 11 columns | 12,575 rows × 11 columns |
| Clean size | **2,252 rows × 12 columns** | **11,362 rows × 18 columns** (after encoding) |
| Main problems | Text-formatted numbers, invalid values in several columns, duplicates, missing values | Missing items, prices and quantities, 33% missing discount flag |
| Focus | Heavy cleaning and feature extraction | Cleaning, encoding and EDA with charts |

## 🧠 Skills demonstrated

- **Data cleaning:** duplicates, missing values, invalid entries, text-to-number conversion
- **Feature engineering:** splitting text columns, label encoding, one-hot encoding, date parts
- **Exploratory data analysis:** summary statistics, distributions, boxplots, group comparisons
- **Data visualization:** Matplotlib and Seaborn
- **Tools:** Python, Pandas, NumPy, Seaborn, Matplotlib, scikit-learn, Jupyter Notebook

---

## 🏠 Notebook 1: Surat Real Estate Listings

**What I did:** took 4,525 messy property listings and cleaned them into 2,252 consistent, numeric, analysis-ready rows.

![Missing values in the raw data](images/nb1_missing_values_raw.png)

### Cleaning log

| Step | What was wrong | Fix | Rows left |
|---|---|---|---|
| 1 | Duplicate listings | Removed 109 duplicates | 4,416 |
| 2 | `price_per_sqft` stored as text like `₹2,891 per sqft` and missing in 8% of rows | Stripped symbols and units, converted to integer, dropped rows without it | 4,058 |
| 3 | `transaction` column held invalid values such as project names and `Unfurnished` | Kept only `New Property` and `Resale` | 3,342 |
| 4 | `status` held values like `21 X 50.2` (plot sizes) | Kept only `Ready to Move` and `Poss. by ...` | 3,288 |
| 5 | `furnishing` held values like `2 Covered` | Kept only `Unfurnished`, `Semi-Furnished`, `Furnished` | 2,951 |
| 6 | `facing` held project names and numbers | Kept only the 8 valid directions | 2,255 |
| 7 | Remaining nulls | Dropped | **2,252** |

![Rows remaining after each step](images/nb1_cleaning_funnel.png)

**Other transformations**
- `square_feet`: `644 sqft` → `644` (numeric)
- `price`: recalculated as `price_per_sqft × square_feet` in rupees
- `floor`: `5 out of 10` → separate `current_floor` and `total_floor` columns
- `property_name` → new `BHK` and `area` columns; dropped the long text `description`
- Encoded `furnishing`, `facing`, `areaWithType` and `transaction` as numbers for modeling

### 💡 Insights (from the 2,252 cleaned listings)

- **3 BHK is the most common listing:** 981 of 2,252 (about 44%).
- **Price per sq ft rises with size:** median ₹3,196 (1 BHK), ₹3,918 (2 BHK), ₹4,700 (3 BHK), ₹5,930 (4 BHK), ₹7,000 (5 BHK).
- **New properties cost about 20% more per sq ft than resale:** median ₹5,000 vs ₹4,170.
- **Median listing price is about ₹69.8 lakh**, and 61% of listings are ready to move.

![Median price per sq ft by BHK](images/nb1_price_per_sqft_by_bhk.png)

---

## 🛒 Notebook 2: Retail Store Sales

**What I did:** cleaned 12,575 retail transactions, encoded the categorical columns and explored spending patterns.

### Cleaning log

| Step | What was wrong | Fix | Result |
|---|---|---|---|
| 1 | Checked for duplicate rows | None found | 12,575 rows |
| 2 | 1,213 rows with missing `Item` (these rows also had the missing prices, quantities and totals) | Dropped them | **11,362 rows**, no nulls in price, quantity or total |
| 3 | `Discount Applied` missing in 3,783 rows (33.3%) | Treated unknown as no discount (0) and noted the assumption | 0 nulls |
| 4 | IDs not useful for analysis | Dropped `Transaction ID` and `Customer ID` | |
| 5 | Text categories | One-hot encoded `Category`; label-encoded `Payment Method` and `Location` | |
| 6 | `Item` like `Item_10_PAT` | Split into `item_code` and `Products` | |
| 7 | `Transaction Date` as text | Split into year, month and day (integers) | |

### 💡 Insights

- **Spending is evenly spread across categories:** Butchers is highest (197K) and Milk Products lowest (171K), a gap of only about 16%.
- **Cash is the most used payment method** (3,917 of 11,362 transactions, about 34%), but the average transaction is almost the same for every method (about 129–131).
- **Online and in-store are split almost 50/50** (5,745 vs 5,617 transactions).
- **Yearly spend dipped about 4% in 2023 and rose about 7% in 2024** (484K → 465K → 499K; data runs from Jan 2022 to mid Jan 2025).
- **Discounts are not tied to price:** the correlation between discount and price per unit is 0.006, and the boxplot below shows the same spread either way.
- **Spend per transaction is right-skewed:** median 108.5, mean 129.6, maximum 410.

![Total spend by category](images/nb2_spend_by_category.png)

![Total spend per year](images/nb2_spend_by_year.png)

| Price per unit by discount status | Distribution of total spent |
|---|---|
| ![Discount vs price](images/nb2_discount_vs_price_boxplot.png) | ![Total spent distribution](images/nb2_total_spent_distribution.png) |

---

## 📁 Repository structure

```
cleaning_datasets/
├── datasets/
│   ├── datasets_1.csv      # Surat real estate data (raw)
│   └── dataset_2.csv       # Retail store sales data (raw)
├── images/                 # Charts used in this README
├── cleaning 1.ipynb        # Notebook 1: real estate cleaning
├── cleaning_2.ipynb        # Notebook 2: retail sales cleaning and EDA
└── README.md
```

## 🚀 Run it yourself

```bash
git clone https://github.com/Shankar112004/cleaning_datasets.git
cd cleaning_datasets
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook
```

**Note on file names:** the notebooks load `surat_uncleaned.csv` (Notebook 1) and `datasets/retail_store_sales.csv` (Notebook 2). These are the files `datasets/datasets_1.csv` and `datasets/dataset_2.csv` in this repo, so update the path in the first `read_csv` cell (or rename the files) before running.

## 🔭 What I would improve next

- Extract the neighbourhood from the Surat listings more reliably, so I can compare prices by area
- Check the real estate data for extreme outliers (for example, very small areas listed with many bedrooms)
- Avoid target leakage when modeling `Total Spent`, since it equals price × quantity
- Fill missing values with group averages instead of dropping rows, then compare the results

## 🔗 More of my work

- [Customer Behavior Analysis](https://github.com/Shankar112004/customer_behavior_analysis): Python + MySQL + Power BI
- [Retail Store Sales Insights](https://github.com/Shankar112004/Retail_Store_Sales): Python + MySQL + Power BI (DAX)
- [Mobile Sales Dashboard](https://github.com/Shankar112004/Model-Sales-Data-): Power BI

---

⭐ If you find this useful, a star is appreciated. Feedback is welcome.
