# Retail Customer Segmentation & Demand Dashboard

**Undergraduate thesis – B.Sc. in Data Science for Business, Universidad de Montevideo (December 2025)**

Exploratory analysis, customer purchase-behavior clustering and a Power BI demand dashboard built on loyalty-program data from one of the largest supermarket chains in Uruguay.

> **Data confidentiality:** the data belongs to the company and is not included in this repository. The company name has been anonymized, and raw data tables, transaction IDs and absolute sales figures were removed from the notebook outputs. The code is shown for reference only and will not run without access to the original data warehouse.

---

## 📌 Business problem

The chain's loyalty program records millions of transactions, together with customer demographics (age, sex, socioeconomic level). The goal of the project was to turn this data into actionable insight:

1. Understand **how customers shop**: what they buy, when, and how this differs across store formats.
2. Explore whether purchase tickets can be grouped into **behavioral segments** to support loyalty and marketing decisions.
3. Build a **dashboard** to monitor demand by store, supplier, product category and sales channel.

## 🗂️ Data

- Source: the company's **Snowflake** data warehouse (direct access granted to the team).
- Modeling sample: **3 stores** representing different formats (hypermarket, traditional supermarket and convenience store), years **2023** (analysis) and **2024** (stability check). About **6.7 million** item-level rows before cleaning and **1.15 million** tickets after cleaning.
- Dashboard: **3 years of sales across 120+ stores**, aggregated from **535 million** rows into a monthly table of about **8 million** rows.

## 🔧 Methodology

**1. Data cleaning**
- Removed records with missing demographics, negative or zero amounts, unrealistic outliers and invalid ages.
- Merged duplicated ticket lines caused by different checkout scanning practices.

**2. Exploratory data analysis**
- Sales seasonality (summer dip, March rebound, December peak).
- Differences in age, socioeconomic level and product mix by store format.
- Customer frequency analysis.

**3. Segmentation**
- Evaluated and discarded **RFM**, **DBSCAN** and **Autoencoders** (with undersampling) because of high noise, computational limits and a ticket-class matrix that was **99.72% sparse**.
- Final approach: **Word2Vec** product embeddings (128 dimensions, context window of 40), aggregated per ticket, then clustered with **K-Means (k = 6)**. Results were evaluated with the Calinski-Harabasz index and the elbow method, and visualized with PCA.

**4. Time-series statistics**
- ADF stationarity test, STL decomposition, t-test on the seasonal component and ANOVA for day-of-week effects.

**5. Power BI dashboard**
- Star-schema model with lookup tables (stores, suppliers, classes and subclasses) and monthly and daily calendars.
- Dynamic DAX measures (units vs. amount, year-over-year comparisons, partial-year view) and hierarchies.

## 📊 Key findings

- **8.5% of customers are frequent shoppers** (at least 12 purchases per year), yet they account for **62% of sales** and **63% of tickets** in the stores analyzed.
- Word2Vec captured intuitive product relationships. For example, tangerines are most similar to oranges (0.87), bananas (0.84) and apples (0.75).
- The data did not show statistically strong clusters (silhouette score of about 0.1), so the 6 segments are presented as **demonstrative profiles**. Even so, they are consistent with business logic:

| Cluster | Profile | Highlights |
|---|---|---|
| 0 | Household & basic groceries | Weekly stock-up, peaks on weekends, middle-income |
| 1 | Quick consumption & convenience | Ready meals, youngest customers, lowest ticket |
| 2 | Impulse & indulgent products | Sweets, soft drinks, snacks |
| 3 | Fresh & healthy food | Highest ticket, older and higher-income customers, highest loyalty rate (12.3%) |
| 4 | Personal care & non-food | Planned purchases, peaks on Fridays |
| 5 | Leisure & social consumption | Beer, snacks, cigarettes; peaks on Saturdays |

- **Store formats behave differently.** The hypermarket drives larger, planned purchases; the traditional supermarket drives replenishment; the convenience store drives impulse and on-the-go purchases.
- **Recommendations:** redesign the loyalty program to reward real recurrence, clean up the product hierarchy, and scale the pipeline to cloud infrastructure to enable churn and recommendation models.

## 🛠️ Tools

Python (pandas, NumPy, scikit-learn, gensim, TensorFlow/Keras, statsmodels, SciPy, matplotlib, seaborn) · SQL · Snowflake · Power BI (DAX) · Jupyter Notebook

## 📁 Repository structure

```
├── notebooks/
│   └── customer_segmentation.ipynb   # EDA, modeling and statistical tests
├── reports/
│   ├── thesis_en.pdf                 # Full thesis (English translation)
│   └── thesis.pdf                    # Full thesis (original, Spanish)
└── README.md
```

## 👥 Authors

Maria Bittencourt · Malena Gómez · **Tomás Sanguinetti** · Paula Planinich

Advisor: Pablo Fleurquin
