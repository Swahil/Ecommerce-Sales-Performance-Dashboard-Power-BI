# Ecommerce-Sales-Performance-Dashboard-Power-BI
Designed to help ecommerce businesses see if they’re truly growing, this dashboard tracks Year-over-Year trends in revenue, sales volume, and customer growth. It also breaks down performance by month, customer, product unit, and manufacturer country in one interactive, filterable view.

![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-0078D4?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)

> An end-to-end Power BI dashboard analysing ecommerce sales performance across customers, products, geographies, and time — with Year-over-Year KPI tracking built using a star-schema data model and DAX.

---

##  Table of Contents

- [Project_Overview](#Project_Overview)
- [Business_Problem](#Business_Problem)
- [Dashboard_Preview](#Dashboard_Preview)
- [Key Metrics and KPIs](#Key_Metrics_and_KPIs)
- [Data_Model](#Data_Model)
- [DAX_Measures](#DAX_Measures)
- [Dashboard_Features](#Dashboard_Features)
- [File_Structure](#File_Structure)
- [How_to_use](#How_to_use)
- [Tools and Technologies](#Tools_and_Technologies)
- [Insights and Findings](#Insights_and_Findings)
- [Recommendations](#Recommendations)
- [Future Enhancements](#Future_Enhancements)
- [Author](#Author)

---

# Project_Overview

This project features an interactive one-page Power BI dashboard built to help ecommerce teams track sales performance, customer behaviour, and product activity in real time. It supports smarter, data-driven decisions by highlighting Year-over-Year (YoY) trends in revenue, sales volume, pricing, and customer growth.

**Target Audience:** Sales Directors, Marketing Analysts, Supply Chain Managers, and C-Suite Executives.

---

# Business_Problem

Ecommerce businesses often struggle to answer questions like:

- Are we growing or declining versus last year?
- Which customers are driving the most revenue?
- Which product units are moving fastest?
- Where in the world are our manufacturers and does geography affect sales?
- How does sales volume trend month-over-month throughout the year?

This dashboard was built to answer all of the above from a single, filterable view.

---

# Dashboard_Preview

> _Open `ECOMMERCE.pbix` in Power BI Desktop to interact with the live dashboard._

**Dashboard includes:**

| Visual | Purpose |
|---|---|
|  Shape Map | Sales/quantity by manufacturer country |
|  Column Chart | Monthly total sales trend |
|  Line Chart | Sales trajectory over months |
|  Donut Chart | Revenue share by top customers |
|  Area Chart | Customer count trend by month |
|  Clustered Bar (x2) | Qty sold by product unit & by country |
|  Year Slicer | Dynamic year filter across all visuals |
|  KPI Cards (x8) | Core metrics + YoY % change indicators |

---

# Key_Metrics_and_KPIs

| KPI | Description |
|---|---|
| **Total Sales** | Sum of all revenue generated |
| **Total Quantity** | Total units sold across all products |
| **Average Unit Price** | Mean selling price per item |
| **Customers** | Count of unique active customers |
| **YoY Sales %** | Year-over-Year revenue growth rate |
| **YoY Quantity %** | Year-over-Year volume growth rate |
| **YoY Customers %** | Year-over-Year customer base growth |

---

# Data_Model

The report is built on a **Star Schema** consisting of:

```
fact_table
    ├── unit                  (product/SKU identifier)
    └── [sales metrics]

customer_dim
    └── name                  (customer name)

item_dim
    └── man_country           (manufacturer country)

Dates
    ├── Month
    └── Year
```

## Table Relationships

```
Dates ──────────────── fact_table (date key)
customer_dim ────────── fact_table (customer key)
item_dim ────────────── fact_table (item key)
```

> The star schema enables fast DAX aggregation and clean cross-filtering between all four dimension tables and the central fact table.

---

# DAX_Measures

All calculated measures live in a dedicated `DAX Measures` table for clean model organisation. Core measures include:

```dax
-- Total Revenue
Total Sales = SUM(fact_table[sales_amount])

-- Total Units Sold
Total Qty = SUM(fact_table[quantity])

-- Average Selling Price
AVG Unit Price = DIVIDE([Total Sales], [Total Qty])

-- Unique Customer Count
Customers = DISTINCTCOUNT(fact_table[customer_id])

-- Year-over-Year Sales Growth
YoY Sales % =
VAR CurrentSales = [Total Sales]
VAR PriorSales = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(Dates[Date]))
RETURN DIVIDE(CurrentSales - PriorSales, PriorSales)

-- Year-over-Year Quantity Growth
YoY Qty % =
DIVIDE(
    [Total Qty] - CALCULATE([Total Qty], SAMEPERIODLASTYEAR(Dates[Date])),
    CALCULATE([Total Qty], SAMEPERIODLASTYEAR(Dates[Date]))
)

-- Year-over-Year Customer Growth
YoY Customers % =
DIVIDE(
    [Customers] - CALCULATE([Customers], SAMEPERIODLASTYEAR(Dates[Date])),
    CALCULATE([Customers], SAMEPERIODLASTYEAR(Dates[Date]))
)
```

> _Note: Exact DAX syntax may vary slightly from the compiled model — these reflect the logical intent of each measure._

---

# Dashboard_Features

- **Dynamic Year Filter** — Slicer on `Dates[Year]` propagates across all 16 visuals simultaneously
- **Cross-Visual Filtering** — Click any bar, slice, or map region to filter the entire dashboard
- **Geographic Mapping** — Custom shape map using a registered GeoJSON resource for manufacturer country visualisation
- **YoY Comparison Cards** — Each KPI card pairs an absolute value with its year-over-year percentage change
- **Drill-Through Ready** — Visual structure supports adding page-level drill-through for deeper product/customer analysis
- **Custom Theme** — Uses Power BI's `CY25SU11` base theme for a consistent, professional colour palette

---

# File_Structure

```
 ecommerce-powerbi-dashboard/
├──  ECOMMERCE.pbix          # Main Power BI report file
├──  data/                   # (Optional) Source data files
│   ├── fact_table.csv
│   ├── customer_dim.csv
│   ├── item_dim.csv
│   └── dates.csv
├──  assets/                 # Screenshots & preview images
│   ├── dashboard_overview.png
│   └── data_model.png
├──  README.md               # Project documentation (this file)

```

---

# How_to_use

## Prerequisites

- [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) (latest version recommended)
- Windows OS (Power BI Desktop is Windows-only)

## Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/Swahil/Ecommerce-Sales-Performance-Dashboard-Power-BI
   ```

2. **Open the report**
   - Launch Power BI Desktop
   - Go to `File → Open report → Browse`
   - Select `ECOMMERCE.pbix`

3. **Refresh data** _(if connecting to a live source)_
   - Go to `Home → Refresh`
   - Update data source credentials under `File → Options → Data source settings`

4. **Explore the dashboard**
   - Use the **Year slicer** to switch between periods
   - Click any visual element to cross-filter
   - Hover over data points for tooltip details

## Publishing to Power BI Service

```
Home → Publish → Select your workspace → Open in Power BI Service
```

---

# Tools_and_Technologies

| Tool | Purpose |
|---|---|
| **Power BI Desktop** | Report authoring & data modelling |
| **DAX** | Calculated measures and KPI logic |
| **Power Query (M)** | Data transformation & cleansing |
| **GeoJSON** | Custom shape map for geographic visuals |
| **Star Schema** | Dimensional data modelling approach |

---

# Insights_&_Findings

> _Replace with findings from your specific dataset. Examples below:_

-  **Revenue Growth:** Total sales grew by **X%** YoY, driven primarily by [top customer segment]
-  **Geographic Concentration:** The majority of manufacturer activity is concentrated in [top countries]
-  **Customer Retention:** Customer count [increased/decreased] by **X%** YoY, suggesting [retention/acquisition trend]
-  **Top-Moving SKUs:** Product units [A, B, C] account for the highest quantity sold
-  **Seasonality:** Peak sales months observed in [Month Range], with a trough in [Month]
-  **Pricing Stability:** Average unit price remained [stable/shifted] at ~[value], indicating [pricing strategy effectiveness]

---

# Recommendations

Based on the dashboard analysis:

1. **Double down on high-value customers** — The donut chart reveals revenue concentration; implement loyalty or account-based marketing for top contributors
2. **Investigate YoY customer decline** (if applicable) — A negative `YoY Customers %` warrants a churn analysis
3. **Optimise slow-moving SKUs** — Units with low `Total Qty` may indicate dead stock or poor product-market fit
4. **Geo-diversify supplier base** — If manufacturer countries are highly concentrated, supply chain risk mitigation may be warranted
5. **Leverage peak season data** — Use monthly trend visuals to plan inventory and campaigns ahead of demand spikes

---

# Future_Enhancements

- [ ] Add a **Product Category** dimension for deeper SKU-level analysis
- [ ] Build a **Drill-Through page** for individual customer or product deep-dives
- [ ] Integrate **Returns & Refunds** data to calculate Net Revenue
- [ ] Add **Profit Margin** measures once cost data is available
- [ ] Connect to a **live database** (SQL Server / Azure) to enable scheduled refresh
- [ ] Create a **mobile-optimised layout** for executive on-the-go access
- [ ] Add **forecast visuals** using Power BI's built-in analytics pane
- [ ] Implement **Row-Level Security (RLS)** for multi-region deployments

---

# Author

**[Benjamin Njoroge]**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=flat&logo=linkedin)](https://linkedin.com/in/your-profile)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=flat&logo=github)](https://github.com/Swahil)

---

# License

This project is licensed under the [MIT License](LICENSE) — free to use, adapt, and share with attribution.

---

