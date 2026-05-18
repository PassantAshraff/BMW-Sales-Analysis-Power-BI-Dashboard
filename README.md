# 🚗 BMW Global Sales Analysis — Power BI Dashboard

A comprehensive Power BI sales analytics project built on BMW's global transaction data (2019–2023), covering **5,000+ records** across 26 car models, 23 countries, 5 regions, and 3 sales channels.

---

## 📁 Project Files

| File | Description |
|---|---|
| `BMW_Sales_Analysis_PBI.pbix` | Power BI Desktop report file containing all visuals, measures, and data model |
| `BMW_Sales_Data.csv` | Raw sales data used as the primary data source |

---

## 📊 Dataset Overview

| Attribute | Details |
|---|---|
| **Time Range** | 2019 – 2023 |
| **Total Records** | 5,000 rows |
| **Models Covered** | 26 BMW models (1 Series → X7, M-series, i-series) |
| **Regions** | Africa, Asia, Europe, North America, South America |
| **Countries** | 23 countries |
| **Sales Channels** | Wholesale, Dealership, Online |

### Columns

| Column | Type | Description |
|---|---|---|
| `Date` | String/Date | Transaction date |
| `Year` | Numeric | Year of the sale |
| `Model` | Text | BMW model name |
| `Revenue` | Numeric | Revenue generated per transaction (USD) |
| `Quantity Sold` | Numeric | Number of units sold per transaction |
| `Region` | Text | Geographic region of the sale |
| `Country` | Text | Country of the sale |
| `Channel` | Text | Sales channel — Wholesale / Dealership / Online |

---

## 🧮 DAX Measures

The following DAX measures were built to power the dashboard's KPIs and visuals:

### 💰 Revenue Measures

| Measure | Purpose |
|---|---|
| **Total Revenue** | Sums all revenue across the entire dataset — the primary KPI of the report |
| **Revenue YTD** | Calculates year-to-date revenue using time intelligence to track progress within a given year |
| **Revenue LY (Last Year)** | Returns total revenue for the same period in the prior year, enabling YoY comparison |
| **Revenue YoY %** | Percentage change in revenue versus the prior year — used in trend analysis cards |
| **Revenue by Model** | Filters total revenue per BMW model to power model-level rankings and comparisons |
| **Revenue by Region** | Aggregates revenue at the region level for geographic breakdown visuals |
| **Revenue by Channel** | Breaks revenue down by sales channel (Wholesale / Dealership / Online) |
| **Avg Revenue per Transaction** | Total Revenue ÷ Number of Transactions — measures the average deal size |

### 📦 Quantity & Volume Measures

| Measure | Purpose |
|---|---|
| **Total Quantity Sold** | Total units sold across all transactions — secondary volume KPI |
| **Avg Quantity per Transaction** | Average units per sale — helps identify bulk vs. individual buying patterns |
| **Units Sold by Model** | Units sold filtered per model — used in volume ranking charts |
| **Units Sold by Region** | Geographic volume distribution of cars sold |

### 📈 Time Intelligence Measures

| Measure | Purpose |
|---|---|
| **Revenue MTD** | Month-to-date revenue accumulation |
| **Revenue QTD** | Quarter-to-date revenue for quarterly performance views |
| **Running Total Revenue** | Cumulative revenue over time — used in area/line charts to show growth trajectory |

### 🏆 Ranking & Comparison Measures

| Measure | Purpose |
|---|---|
| **Model Rank by Revenue** | Dynamic ranking of BMW models by total revenue using `RANKX` — updates with slicer selections |
| **Top N Models Revenue** | Filtered measure to show only the top N models (used with a What-If parameter) |
| **Country Rank** | Ranks countries by revenue contribution within each region |

---

## 🔑 Key Insights

- **Total Revenue (2019–2023):** ~$376M across all channels and regions
- **Top Channel:** Wholesale leads with ~$165.8M (44% of total revenue)
- **Top Region:** Africa generated the highest revenue (~$82.3M), followed by South America and Asia
- **Top Model:** BMW Z4 leads all models at ~$17.1M, followed by the 3 Series and X4
- **Revenue is consistent year-over-year**, peaking in 2021 (~$77M) with slight variations between years
- **Average revenue per transaction:** ~$75,213
- **Average units per transaction:** ~3 units

---

## 🗺️ Dashboard Pages (Report Structure)

1. **Executive Summary** — High-level KPIs: Total Revenue, Total Quantity Sold, YoY Growth
2. **Sales by Region & Country** — Map visual and bar charts breaking down performance geographically
3. **Model Performance** — Rankings, revenue, and quantity by BMW model
4. **Channel Analysis** — Comparison of Wholesale vs. Dealership vs. Online channels
5. **Time Trends** — Year-over-year and monthly trend lines with time intelligence measures

---

## 🛠️ Tools & Technologies

- **Power BI Desktop** — Data modeling, DAX, and report authoring
- **DAX (Data Analysis Expressions)** — All calculated measures and KPIs
- **CSV** — Raw data source
- **Power Query (M)** — Data cleaning and transformation

---

## 🚀 How to Use

1. Clone or download this repository
2. Open `BMW_Sales_Analysis_PBI.pbix` in **Power BI Desktop**
3. If prompted, refresh the data source and point it to `BMW_Sales_Data.csv` in the same directory
4. Explore the dashboard using the slicers (Year, Region, Model, Channel) to filter views dynamically



