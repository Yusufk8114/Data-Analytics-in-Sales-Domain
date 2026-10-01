# Data-Analytics-in-Sales-Domain
# USA Regional Sales Analysis

An end-to-end sales analytics project that consolidates a multi-table regional sales dataset, engineers profitability metrics in Python, and delivers findings through an interactive four-page Power BI dashboard.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Business Objectives](#business-objectives)
3. [Repository Structure](#repository-structure)
4. [Data Description](#data-description)
5. [Methodology](#methodology)
6. [Dashboard](#dashboard)
7. [Key Findings](#key-findings)
8. [Recommendations](#recommendations)
9. [Getting Started](#getting-started)
10. [Data Notes and Limitations](#data-notes-and-limitations)
11. [Author](#author)

---

## Project Overview

| Metric | Value |
|---|---|
| Analysis period | January 2014 – February 2018 |
| Order lines | 64,104 |
| Unique orders | 10,684 |
| Customers | 175 |
| Products | 30 |
| Geographic coverage | 47 US states and Washington, DC, grouped into 4 regions |
| Total revenue | ≈ $1.236 billion |
| Total profit | ≈ $461.8 million |
| Overall profit margin | ≈ 37.4% |

The project follows a standard analytics workflow: ingestion, validation, transformation, exploratory analysis, and business reporting.

## Business Objectives

- Quantify revenue, profit, and margin performance across time, geography, channel, product, and customer.
- Identify seasonality and long-term revenue trends.
- Determine which regions, states, products, and customers contribute most to revenue and profitability.
- Evaluate whether pricing is related to profitability.
- Provide stakeholders with a self-service dashboard for filtering and drill-down.

## Repository Structure

```
.
├── Regional_Sales_Dataset.xlsx      # Raw source data (6 sheets)
├── Regional_Sales_Analysis.ipynb    # Cleaning, feature engineering, and EDA
├── Sales_data_csv.xlsx              # Cleaned, analysis-ready dataset (64,104 rows × 22 columns)
├── SALES_REPORT.pbix                # Power BI report (4 pages)
└── README.md
```

## Data Description

The raw workbook follows a star-style layout: one fact sheet and five dimension/reference sheets.

| Sheet | Rows | Description |
|---|---|---|
| Sales Orders | 64,104 | Fact table: order number and date, channel, warehouse, quantity, unit price, line total, unit cost |
| Customers | 175 | Customer index and customer name |
| Products | 30 | Product index and product name |
| Regions | 994 | US cities with county, state, coordinates, population, households, median income, time zone |
| State Regions | 49 | State code, state name, and US region (Northeast, Midwest, South, West) |
| 2017 Budgets | 30 | 2017 budget target per product |

## Methodology

### 1. Ingestion and Profiling
All sheets were loaded with `pandas`. Shape, null-value, and duplicate checks confirmed the sales table has no missing values and no duplicate records.

### 2. Cleaning and Integration
- Joined the sales fact table with customer, product, region, state-region, and budget tables.
- Corrected the State Regions sheet, whose header had been stored as a data row.
- Removed redundant key columns, standardised column names to `snake_case`, and retained 15 analytical columns.
- Set the 2017 budget to null for orders outside 2017, since the budget applies to that year only.

### 3. Feature Engineering

| Feature | Definition |
|---|---|
| `total_cost` | `quantity × cost` |
| `profit` | `revenue − total_cost` |
| `profit_margin_pct` | `profit ÷ revenue × 100` |
| `order_month_name`, `order_month_num` | Month attributes derived from `order_date` |

### 4. Exploratory Data Analysis
Conducted with Matplotlib, Seaborn, and Plotly:

- Monthly sales trend and seasonality profile (excluding partial-year 2018)
- Top products by revenue and by average profit
- Revenue by channel, region, and state (including a choropleth map)
- Order value distribution and unit price versus margin analysis
- Top and bottom customers; top states by revenue and order count
- Customer segmentation by revenue, margin, and order volume
- Correlation analysis of numeric features

### 5. Business Intelligence Reporting
The cleaned dataset was loaded into Power BI to build an interactive report with shared slicers and KPI cards.

## Dashboard

`SALES_REPORT.pbix` contains four pages with consistent navigation and slicers for **Year, Month, Region, and Channel**. Each analytical page includes a *Clear all slicers* control.

| Page | Contents |
|---|---|
| Home | Landing page and navigation |
| Executive Overview and Trends | KPI cards (Total Revenue, Total Profit, Profit Margin %, Total Orders, Revenue per Order); monthly revenue and profit trends; order value distribution; unit price versus margin |
| Product and Channel Performance | Revenue, profit, and margin by channel; top products by revenue and margin; revenue versus profitability positioning |
| Geographic and Customer Insights | Revenue by region and state (top and bottom 5); top and bottom 5 customers by revenue and margin; budget share and margin by region |

> **Preview:** add dashboard screenshots to a `/images` folder and reference them here, e.g. `![Executive Overview](images/executive_overview.png)`.

## Key Findings

All figures are derived from `Sales_data_csv.xlsx`.

**Performance and trend**
- Annual revenue is stable at roughly **$294–298 million** from 2014 to 2017, indicating a mature, plateaued business. 2018 covers January and February only.
- **January is the peak month** (~$124 million across all years), followed by February (~$115 million). Monthly revenue then stabilises around $95–102 million.

**Geography**
- **California** contributes **$228.8 million (~18.5%)** of total revenue, more than twice second-ranked Illinois (~$111 million). Florida, Texas, and New York complete the top five.
- By region, the **West leads at ~30%**, followed by the South (~27%), Midwest (~26%), and Northeast (~17%).
- Regional margins are tightly clustered between 37.1% and 37.5%.

**Channels**
- **Wholesale** generates ~54% of revenue (5,766 orders), followed by **Distributor** at ~31% and **Export** at ~15%.
- Margins are similar across channels (37.0%–38.0%), with Export marginally highest.

**Products**
- **Product 26** (~$117 million) and **Product 25** (~$109 million) are the top revenue drivers.
- **Product 9** has the highest margin (~40.0%); **Product 12** has the lowest (~34.4%).

**Customers**
- Top accounts by revenue are **Aibox Company** (~$12.6 million), **State Ltd** (~$12.2 million), and **Pixoboo Corp** (~$11.0 million).

**Pricing**
- Unit price shows **no meaningful correlation with profit margin** (r ≈ −0.005), so higher-priced items are not inherently more profitable.

## Recommendations

The following are suggestions based on the observed patterns and would benefit from validation with business context:

1. **Protect and grow the core markets.** California and the leading states account for a disproportionate share of revenue; account coverage there should be prioritised.
2. **Prepare for the Q1 peak.** January and February demand is materially above the annual average; inventory and staffing plans should reflect this.
3. **Review product mix for margin.** Shifting emphasis toward higher-margin products (e.g., Product 9) and examining cost drivers for lower-margin products (e.g., Product 12) could lift overall profitability.
4. **Decouple pricing from assumed profitability.** Because price and margin are uncorrelated, pricing decisions should be informed by product-level cost data.
5. **Explore growth outside the plateau.** Flat annual revenue suggests incremental gains may come from underpenetrated states and the Export channel.

## Getting Started

**Prerequisites:** Python 3.8+, Jupyter, and Power BI Desktop (Windows) for the dashboard.

```bash
# Clone the repository
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# Install dependencies
pip install pandas numpy matplotlib seaborn plotly openpyxl jupyter

# Launch the notebook
jupyter notebook Regional_Sales_Analysis.ipynb
```

1. Ensure the raw workbook is in the project root. The notebook expects the filename `Regional Sales Dataset.xlsx`; update the path in the ingestion cell if your file is named `Regional_Sales_Dataset.xlsx`.
2. Run all cells. The notebook exports the cleaned data as `Sales_data(EDA Exported).csv`.
3. Open `SALES_REPORT.pbix` in Power BI Desktop. If prompted, point the data source to the cleaned dataset.

## Data Notes and Limitations

- **Partial final year.** 2018 includes only January 1 – February 28; exclude it from year-over-year comparisons.
- **Budget coverage.** Budgets exist for 2017 only, at product level and annual granularity, so budget-versus-actual analysis by region or month is not possible.
- **Synthetic data.** Generic product and company names indicate a sample dataset; findings illustrate analytical technique rather than a real business.
- **Currency.** All transactions are recorded in USD.
