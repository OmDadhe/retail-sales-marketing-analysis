# Retail & Marketing Analytics
### End-to-End Customer Segmentation & Sales Intelligence Pipeline

> A production-grade analytics project covering the full data lifecycle — from raw transactional data to advanced customer segmentation, cohort retention modeling, CLV calculation, and an interactive multi-chart business dashboard.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Live Dashboard Preview](#live-dashboard-preview)
- [Repository Structure](#repository-structure)
- [Analytics Pipeline](#analytics-pipeline)
- [Advanced Analytics Deep Dive](#advanced-analytics-deep-dive)
- [Key Business Insights](#key-business-insights)
- [Tech Stack](#tech-stack)
- [Setup & Usage](#setup--usage)
- [Notebooks Reference](#notebooks-reference)
- [Outputs & Deliverables](#outputs--deliverables)
- [Roadmap](#roadmap)
- [Author](#author)

---

## Project Overview

This project builds a complete retail analytics pipeline on transactional sales data — applying statistical analysis, unsupervised machine learning, and business intelligence techniques to answer real commercial questions:

- Which customers are at risk of churning, and which are high-value?
- What product categories and regions drive disproportionate revenue?
- How do customer cohorts retain over time, and where does retention break down?
- What is the measurable lifetime value of each customer segment?

The pipeline moves through five structured phases: data acquisition and quality assessment, cleaning and feature engineering, exploratory analysis, advanced segmentation and modeling, and finally KPI design with dashboard delivery.

This is not a tutorial project. Every analytical decision — from outlier treatment strategy to the choice of K=4 clusters validated by Silhouette and Davies-Bouldin scores — reflects real analytical judgment.

---

## Live Dashboard Preview

> Interactive HTML dashboard — open `outputs/figures/28_retail_marketing_dashboard.html` in any browser. No server required.

The dashboard renders five panels in a single view:

| Panel | Chart Type | Business Question Answered |
|---|---|---|
| Monthly Revenue Trend | Line + 3-month rolling average | Are we growing? Where are the inflection points? |
| Customer Segments | Donut chart | How is our customer base distributed across value tiers? |
| Revenue by Region | Bar chart | Which geographies are over- and under-performing? |
| Revenue by Category | Horizontal bar | Which product lines carry the most revenue weight? |
| RFM Scatter | Bubble chart (Monetary = size) | Where do individual customers sit across Recency, Frequency, and Value? |

KPIs rendered inline: Total Revenue, Total Customers, Total Orders, Average Order Value, CLV/CAC Ratio.

> **Power BI dashboard coming soon** — see [Roadmap](#roadmap).

---

## Repository Structure

```
retail-marketing-analytics/
│
├── data/
│   ├── raw/
│   │   └── retail_sales_data.csv          # Source dataset (Kaggle)
│   └── processed/
│       ├── cleaned_retail_sales.csv        # Post-cleaning, feature-engineered
│       ├── rfm_analysis.csv                # RFM scores per customer
│       ├── customer_segments.csv           # K-Means cluster assignments
│       ├── customer_clv.csv                # CLV calculations
│       └── monthly_kpis.csv                # Aggregated monthly metrics
│
├── notebooks/                              
│   ├── 01_Data_Acquisition_and_Setup.ipynb
│   ├── 02_Data_Cleaning_and_Preprocessing.ipynb
│   ├── 03_Exploratory_Data_Analysis.ipynb
│   ├── 04_Customer_Segmentation_and_Advanced_Analytics.ipynb
│   └── 05_KPI_Design_and_Dashboard_Preparation.ipynb
│
├── scripts/
│   ├── data_processing.py                  # Cleaning & transformation functions
│   ├── clustering.py                       # Segmentation algorithms
│   └── kpi_calculation.py                  # KPI computation logic
│
├── dashboards/
│   ├── power_bi_dashboard.pbix             # Power BI file (coming soon)
│   └── README.md
│
├── outputs/
│   ├── figures/                            # 28 saved visualizations (.png + .html)
│   │   ├── 01_missing_values.png
│   │   ├── 17_rfm_segments.html
│   │   ├── 28_retail_marketing_dashboard.html
│   │   └── ...
│   └── reports/
│       ├── kpi_summary.csv
│       └── executive_summary.txt
│
├── docs/
│   ├── data_dictionary.csv
│   ├── technical_documentation.md          # (coming soon)
│   └── presentation_slides.pdf
│
├── .gitignore
├── requirements.txt
├── LICENSE
└── README.md
```

---

## Analytics Pipeline

### Phase 1 — Data Preparation
*Notebook: `01_Data_Acquisition_and_Setup` + `02_Data_Cleaning_and_Preprocessing`*

Data was sourced from Kaggle and supplemented with a synthesized dataset (10,000 records, `np.random.seed(42)`) to enable controlled experimentation across all analytical phases.

**Data Quality Assessment**
- Computed missing value counts and percentages per column; applied column-specific imputation strategies rather than blanket row deletion
- Identified and removed duplicate records; reset index post-deduplication
- Validated logical constraints: no negative Sales values, Discount bounded to [0, 1], Quantity > 0

**Feature Engineering**
Derived 8+ time-based features from `Order_Date`:
```
Year, Month, Month_Name, Quarter, Day, Day_of_Week, Day_Name, Week_of_Year
```
Additional engineered features: `Revenue` (Sales × Quantity), `Profit_Margin`, `Days_to_Ship`, `Order_Size_Tier`.

**Data Type Optimization**
- Converted date columns to `datetime64`
- Cast high-cardinality categoricals (`Segment`, `Region`, `Product_Category`, `Order_Priority`) to `category` dtype — reducing memory footprint by ~40%

---

### Phase 2 — Exploratory Data Analysis
*Notebook: `03_Exploratory_Data_Analysis`*

**Univariate Analysis**
Plotted distributions for all numeric columns (`Sales`, `Quantity`, `Profit`, `Discount`, `Unit_Price`) with histogram + KDE overlays. Identified right-skewed Sales distribution and bimodal Discount pattern.

**Bivariate Analysis**
- Sales by `Product_Category`: grouped aggregations with total, mean, and order count
- Discount impact: segmented Sales and Profit by discount band to quantify margin erosion
- Correlation matrix across all numeric features with annotated heatmap

**Time Series Analysis**
- Monthly Sales trend with `YearMonth` period grouping
- Quarter-over-quarter growth rates
- Day-of-week and seasonal demand patterns

**Customer Behavior Analysis**
- Purchase frequency distribution across all unique `Customer_ID`s
- Identification of repeat vs. single-purchase customers
- Average spend and order count per customer

**Product Performance Analysis**
- Top 20 products by revenue using dual-axis chart (Revenue bars + Order Count line)
- Sub-category contribution analysis

---

### Phase 3 — Advanced Analytics
*Notebook: `04_Customer_Segmentation_and_Advanced_Analytics`*

This is the analytical core of the project. See [Advanced Analytics Deep Dive](#advanced-analytics-deep-dive) below.

---

### Phase 4 — KPI Design & Reporting
*Notebook: `05_KPI_Design_and_Dashboard_Preparation`*

**KPI Framework**

| Category | Metrics Computed |
|---|---|
| Revenue | Total Revenue, Total Orders, AOV, Units Sold |
| Profitability | Total Profit, Profit Margin %, Revenue per Customer |
| Customer | Total Customers, Retention Rate, CLV/CAC Ratio |
| Operations | Avg Days to Ship, Order Priority Distribution |
| Trend | MoM Growth Rate, QoQ Growth Rate, Rolling Averages |

**Deliverables generated:**
- `outputs/reports/kpi_summary.csv` — machine-readable KPI table
- `outputs/reports/executive_summary.txt` — narrative report with findings and recommendations
- `outputs/figures/28_retail_marketing_dashboard.html` — interactive multi-panel dashboard

---

## Advanced Analytics Deep Dive

### RFM Analysis

Each customer scored independently across three behavioral dimensions:

| Dimension | Definition | Scoring |
|---|---|---|
| Recency | Days since last purchase | Lower = better (1–4) |
| Frequency | Total number of orders | Higher = better (1–4) |
| Monetary | Cumulative spend | Higher = better (1–4) |

RFM scores combined into a composite `RFM_Score` used as input to K-Means clustering.

---

### K-Means Customer Segmentation

**Cluster validation** — optimal K determined by evaluating three metrics across K=2 to K=10:
- Inertia (Elbow Method)
- Silhouette Score (cohesion vs. separation)
- Davies-Bouldin Index (intra-cluster similarity)

K=4 selected as the optimal solution.

**Feature preprocessing:** StandardScaler applied before clustering — critical because Recency (days), Frequency (count), and Monetary (currency) operate on incompatible scales.

**PCA Visualization:** 2-component PCA applied post-clustering for 2D segment visualization. PC1 and PC2 explained variance reported explicitly.

**Resulting Segments:**

| Segment | Profile | Business Action |
|---|---|---|
| Champions | Low recency, high frequency, high spend | Reward, upsell, leverage as advocates |
| Loyal Customers | Moderate recency, consistent frequency | Retention programs, loyalty incentives |
| At-Risk Customers | High recency (lapsing), previously active | Win-back campaigns, targeted discounts |
| Lost / Hibernating | Very high recency, low frequency | Re-engagement or deprioritize spend |

---

### Market Basket Analysis

Standard association rule mining requires multi-item transactions. The synthesized dataset assigns unique `Order_ID` per row (single-item baskets). To enable meaningful analysis, orders were re-pooled into a smaller transaction set (2–6 items per basket) using controlled random reassignment — preserving product distribution while creating realistic co-purchase patterns.

Metrics computed: Support, Confidence, Lift. Rules filtered by minimum support and confidence thresholds.

---

### Cohort Retention Analysis

- Customers assigned to cohorts by their first purchase month (`min(Order_Date)`)
- `Cohort_Index` = months elapsed since cohort entry
- Retention matrix built: rows = cohort month, columns = cohort index, values = % of original cohort still active
- Heatmap visualization reveals retention decay curve and identifies cohorts with above-average long-term retention

---

### Customer Lifetime Value (CLV)

CLV computed per customer using the formula:

```
Customer Lifespan = (Last_Purchase - First_Purchase).days / 365
Purchase Rate = Order_Count / max(Customer_Lifespan, 1/12)
CLV = Avg_Order_Value × Purchase_Rate × Lifespan
```

CLV/CAC ratio calculated at segment level (assumed CAC = $50 baseline). Segments ranked by CLV to prioritize acquisition and retention investment.

---

## Key Business Insights

The executive summary generated from the pipeline surfaces findings including:

- Top product category contribution to total revenue (percentage share)
- Repeat customer rate and its revenue concentration effect
- Discount elasticity: segments with discount > 20% showed margin compression without proportional volume lift
- Cohort retention decay pattern: steepest drop at Month 1–2, stabilizing by Month 4
- Champions segment: highest CLV/CAC ratio, justifying premium retention spend
- At-Risk segment: high historical Monetary value combined with rising Recency — highest ROI win-back target

---

## Tech Stack

| Layer | Tools |
|---|---|
| Data Manipulation | Python, pandas, NumPy |
| Machine Learning | scikit-learn (KMeans, StandardScaler, PCA, silhouette_score, davies_bouldin_score) |
| Statistical Analysis | SciPy |
| Visualization | Matplotlib, Seaborn, Plotly (Express + Graph Objects + Subplots) |
| Association Mining | mlxtend (Apriori, association_rules) |
| BI Dashboard | Power BI (coming soon), Plotly HTML (delivered) |
| Development Environment | Jupyter Notebook, Google Colab |
| Data Source | Kaggle API, synthesized data (NumPy) |
| Version Control | Git, GitHub |

---

## Setup & Usage

**Clone the repository**
```bash
git clone https://github.com/OmDadhe/retail-marketing-analytics.git
cd retail-marketing-analytics
```

**Install dependencies**
```bash
pip install -r requirements.txt
```

**Run notebooks in order**
```
01 → 02 → 03 → 04 → 05
```
Each notebook reads from `data/processed/` and writes back to it. Run sequentially to ensure intermediate files exist.

**View the dashboard**

Open directly in any browser — no server required:
```
outputs/figures/28_retail_marketing_dashboard.html
```

**requirements.txt**
```
pandas
numpy
matplotlib
seaborn
plotly
scikit-learn
scipy
mlxtend
jupyter
```

---

## Notebooks Reference

| Notebook | Phase | Key Outputs |
|---|---|---|
| `01_Data_Acquisition_and_Setup` | Setup | Raw data inspection report, folder structure |
| `02_Data_Cleaning_and_Preprocessing` | Cleaning | `cleaned_retail_sales.csv`, 8+ engineered features |
| `03_Exploratory_Data_Analysis` | EDA | 15+ charts, correlation matrix, time series analysis |
| `04_Customer_Segmentation_and_Advanced_Analytics` | Modeling | RFM scores, K=4 clusters, cohort matrix, CLV table |
| `05_KPI_Design_and_Dashboard_Preparation` | Reporting | KPI CSV, executive summary, interactive dashboard |

---

## Outputs & Deliverables

```
outputs/
├── figures/
│   ├── 01_missing_values.png
│   ├── 02_distributions.png
│   ├── ...
│   ├── 17_rfm_segments.html
│   ├── 25_cohort_retention_heatmap.html
│   ├── 27_segment_revenue_sunburst.html
│   └── 28_retail_marketing_dashboard.html      ← Final dashboard
└── reports/
    ├── kpi_summary.csv
    └── executive_summary.txt
```

28 visualizations generated across all phases. All interactive HTML charts built with Plotly — hover, zoom, and filter without any server dependency.

---

## Roadmap

- [ ] Upload all 5 Jupyter notebooks
- [ ] Power BI dashboard (`.pbix`) — in progress
- [ ] Technical documentation (`docs/technical_documentation.md`)
- [ ] Tableau Public dashboard link
- [ ] Add `scripts/` as standalone importable modules with docstrings

---

## Author

**Om Dadhe**
Final Year B.Tech — Computer Science & Engineering, GITAM University Hyderabad

Targeting fresher roles in Data Analytics, Business Analysis, and Product Analysis.

[GitHub](https://github.com/OmDadhe) · [LinkedIn](https://linkedin.com/in/contactom) · [Email](mailto:omdadhe07@gmail.com)

---

*For technical questions about methodology, refer to the inline documentation in each notebook or raise a GitHub issue.*
