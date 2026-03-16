# outputs/figures/

This folder contains all 28 visualizations generated across the five analysis phases of the Retail & Marketing Analytics project. Static charts are saved as high-resolution PNG (300 DPI). Interactive charts are saved as standalone HTML files — open in any browser with no server required.

---

## Phase 1 — Data Acquisition & Setup

| File | Type | Description |
|---|---|---|
| `01_missing_values.png` | PNG | Bar chart showing the count of missing values per column across the raw dataset. Used to prioritize imputation strategy — numerical columns filled with median, categorical with mode. |
| `02_data_types_distribution.png` | PNG | Pie chart showing the proportion of column data types (float64, int64, object, datetime). Used to confirm dtype mix before type conversion. |

---

## Phase 2 — Data Cleaning & Preprocessing

| File | Type | Description |
|---|---|---|
| `03_outliers_before_treatment.png` | PNG | Side-by-side boxplots for `Sales`, `Quantity`, and `Profit` before outlier treatment. Shows the spread and extent of extreme values identified via IQR method. |
| `04_outliers_after_treatment.png` | PNG | Same three boxplots post-Winsorization. Values outside `[Q1 - 1.5×IQR, Q3 + 1.5×IQR]` were clipped. Confirms that outlier influence was reduced without removing any records. |

---

## Phase 3 — Exploratory Data Analysis

| File | Type | Description |
|---|---|---|
| `05_numerical_distributions.png` | PNG | 2×3 histogram grid for `Sales`, `Quantity`, `Profit`, `Discount`, and `Unit_Price`. Each panel includes mean (red) and median (green) reference lines. Reveals right-skewed Sales distribution and bimodal Discount pattern. |
| `06_categorical_distributions.png` | PNG | 2×3 bar chart grid for `Segment`, `Region`, `Product_Category`, `Order_Priority`, and `Season`. Value count labels on each bar. Shows category balance across key dimensions. |
| `07_sales_by_category.html` | HTML | Interactive bar chart — total revenue by `Product_Category`, color-scaled by value. Hover shows exact revenue per category. |
| `08_sales_by_region.html` | HTML | Interactive donut chart — revenue share by `Region`. Hole=0.4 for readability. Shows geographic revenue distribution at a glance. |
| `09_correlation_matrix.png` | PNG | Annotated heatmap of Pearson correlation coefficients between `Sales`, `Quantity`, `Profit`, and `Discount`. `cmap='coolwarm'` centered at 0. Highlights the discount-to-profit relationship. |
| `10_monthly_sales_trend.html` | HTML | Interactive line chart — total monthly revenue over the full analysis period. Unified hover mode. Reveals seasonality and growth trajectory. |
| `11_quarterly_comparison.html` | HTML | Grouped bar chart — quarterly revenue broken down by year. Shows year-over-year quarterly performance side by side. |
| `12_sales_by_day.html` | HTML | Bar chart — average sales per day of week (Monday–Sunday). Identifies intra-week demand patterns and weekend lift. |
| `13_customer_frequency.png` | PNG | Histogram of purchase frequency distribution across all unique customers. Mean and median reference lines included. Shows the concentration of one-time vs. repeat buyers. |
| `14_top_customers.html` | HTML | Color-scaled horizontal bar chart — top 10 customers ranked by total lifetime revenue. Useful for identifying high-value account concentration. |
| `15_top_products.html` | HTML | Dual-axis chart — top 20 products by revenue (bars, left Y-axis) overlaid with total quantity sold (line, right Y-axis). Separates high-revenue from high-volume products. |
| `16_category_performance.html` | HTML | Bubble scatter — x=Order Count, y=Average Order Value, bubble size=Total Revenue, color=Product Category. Reveals the volume-vs-value trade-off across categories. |

---

## Phase 4 — Advanced Analytics & Segmentation

| File | Type | Description |
|---|---|---|
| `17_rfm_segments.html` | HTML | Donut chart — customer count distribution across 10 rule-based RFM segments (Champions, Loyal Customers, At Risk, Lost, etc.). Derived from R/F/M score threshold logic. |
| `18_optimal_clusters.png` | PNG | 3-panel plot used to select optimal K for K-Means: (1) Elbow/Inertia curve, (2) Silhouette Score by K, (3) Davies-Bouldin Index by K. K selected at the maximum Silhouette Score. |
| `19_customer_segments_pca.html` | HTML | 2D PCA scatter — each customer plotted on PC1 vs PC2 axes, colored by K-Means cluster name. Axis labels show explained variance per component. Hover reveals individual RFM values. |
| `20_customer_segments_3d.html` | HTML | 3D scatter in raw RFM space — Recency × Frequency × Monetary — colored by cluster. Allows inspection of cluster separation across all three dimensions simultaneously. |
| `21_segment_distribution.html` | HTML | Side-by-side bar charts — left: customer count per segment, right: total revenue per segment. Shows that customer count and revenue contribution are not always proportional. |
| `22_association_rules_category.html` | HTML | Scatter plot of association rules at Product Category level — x=Support, y=Confidence, bubble size=Lift, color=Lift. Hover shows antecedent → consequent pairs. Rules filtered by Lift > 1 and Confidence ≥ 0.3. |
| `22_association_rules_sub_category.html` | HTML | Same association rules scatter at Product Sub-Category level (8 items). Lower min_support threshold (2%) applied due to higher item cardinality. Reveals granular cross-sell patterns. |
| `23_cohort_retention.png` | PNG | Heatmap — rows=cohort acquisition month, columns=months since first purchase (Cohort Index), values=retention rate %. `cmap='YlGnBu'`, scale 0–100%. Shows where retention decays fastest and which cohorts have above-average longevity. |
| `24_clv_distribution.html` | HTML | Histogram of Customer Lifetime Value (CLV) distribution across all customers. Median reference line included. CLV computed as `AOV × Purchase Rate × 3-year assumed lifespan`. |

---

## Phase 5 — KPI Design & Dashboard Preparation

| File | Type | Description |
|---|---|---|
| `25_monthly_revenue_trend_detailed.html` | HTML | Detailed monthly revenue line chart with `plotly_white` template. Line + markers, unified hover. Used as the basis for the KPI reporting section. |
| `26_mom_kpi_comparison.html` | HTML | Grouped bar chart comparing the latest month vs. the previous month across Revenue, Orders, Customers, and AOV. Highlights month-over-month business momentum. |
| `27_segment_revenue_sunburst.html` | HTML | Sunburst chart — segment path, sized by total revenue, colored by average purchase frequency using `RdYlGn` scale. Shows both revenue weight and purchase behavior intensity per segment. |
| `28_retail_marketing_dashboard.html` | HTML | **Final consolidated dashboard.** 5-panel interactive layout: Monthly Revenue Trend (with 3-month rolling average), Customer Segments donut, Revenue by Region, Revenue by Category (horizontal), and RFM Scatter (bubble size = Monetary). KPI bar pinned below title: Total Revenue, Customers, Orders, AOV, CLV/CAC. White background, no server required. |

---

## Notes

- All HTML files are fully self-contained — Plotly JS is bundled inline. No internet connection needed to view them.
- All PNG files are saved at 300 DPI with `bbox_inches='tight'` for clean export.
- Figure numbering is sequential across all five notebooks — `01_` through `28_` — reflecting the order of generation in the analysis pipeline.
- Figure `22` generates two files (category and sub-category level) from a single dynamic save loop.
