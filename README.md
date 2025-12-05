# Vendor Performance Data Analysis

Vendor Performance Dashboard 
<img width="767" height="462" alt="Screenshot 2025-12-04 at 5 03 25 PM" src="https://github.com/user-attachments/assets/efb642f0-2f18-4f80-a9fa-23de912515c2" />


## Overview
Analysis of vendor performance data to optimize profitability and pricing strategies across a liquor inventory management system. Processed **15.4M+ records** from 2024 to generate actionable business insights.

**Key Metrics:** $441.41M Sales | $307.34M Purchases | $134.07M Profit | 38.7% Margin | $2.71M Unsold Inventory

## Business Problem

Optimize profitability through better inventory and vendor management by:
- Identifying underperforming brands requiring promotional adjustments
- Determining top vendors contributing to sales and profit
- Analyzing bulk purchasing impact on costs
- Assessing inventory turnover to reduce holding costs
- Investigating profitability variance between vendor tiers

## Project Methodology & Analysis

### 1. Data Pipeline Development
- Designed and implemented automated ETL pipeline using Python
- Created `ingestion_db.py` to batch process 6 CSV files into SQLite database
- Built `get_vendor_summary.py` for data aggregation and metric calculation
- Implemented comprehensive logging system for monitoring and debugging

### 2. Database Design & Optimization
- Structured SQLite database to handle 15.4M+ records efficiently
- Designed complex SQL queries using CTEs (Common Table Expressions) for multi-table joins
- Created `vendor_sales_summary` table as single source of truth
- Optimized query performance through pre-aggregation (225x faster dashboard queries)

### 3. Data Cleaning & Transformation
- Handled data quality issues:
  - Converted Volume column from object to float datatype
  - Managed 178 missing values for unsold products
  - Stripped whitespace from 126 vendor names
  - Filtered negative profits and zero-sales records
- Standardized measurement units (volumes in mL, currency in USD)
- Validated data integrity across table relationships

### 4. Exploratory Data Analysis
- Conducted comprehensive EDA on 7 database tables
- Performed deep-dive analysis on sample vendor (#4466) to validate data patterns
- Analyzed distributions of 18 variables using histograms
- Identified outliers and data anomalies (negative profits, extreme freight costs)
- Examined purchasing patterns across vendors and brands

### 5. Statistical Analysis
- **Descriptive Statistics**: Calculated mean, median, std dev for all metrics
- **Correlation Analysis**: Built correlation heatmap showing relationships between 18 variables
  - Discovered strong correlation (0.999) between purchase and sales quantities
  - Found weak negative correlation between profit margin and sales price (-0.179)
- **Hypothesis Testing**: 
  - Tested difference in profit margins between top and low vendors
  - Used 95% confidence intervals
  - Rejected null hypothesis with statistical significance
  - Top vendors: 31.17% (CI: 30.74-31.61%) vs Low vendors: 41.55% (CI: 40.48-42.62%)
- **Outlier Detection**: Identified extreme values in pricing, freight costs, and stock turnover

### 6. Business Metrics Engineering
Created 4 key performance indicators:
```python
GrossProfit = TotalSalesDollars - TotalPurchaseDollars
ProfitMargin = (GrossProfit / TotalSalesDollars) × 100
StockTurnover = TotalSalesQuantity / TotalPurchaseQuantity
SalesToPurchaseRatio = TotalSalesDollars / TotalPurchaseDollars
```

### 7. Business Intelligence & Insights
- Identified 198 high-margin, low-sales brands for promotional campaigns
- Quantified vendor concentration risk (65.69% dependency on top 10)
- Calculated bulk purchasing advantages (72% cost savings for large orders)
- Discovered $2.71M in unsold inventory requiring optimization
- Segmented vendors into performance tiers with distinct profitability models

### 8. Data Visualization
- **Power BI Dashboard**: Built interactive dashboard with 5 visualizations
  - KPI cards showing key business metrics
  - Donut chart for vendor purchase contribution
  - Horizontal bar charts for top vendors and brands
  - Scatter plot identifying low-performing brands
  - Color-coded performance indicators
- **Python Visualizations**: Created distribution plots, correlation heatmaps, confidence interval charts

### 9. Reporting & Documentation
- Developed comprehensive business analysis report with findings and recommendations
- Created technical documentation for reproducibility
- Documented data pipeline architecture
- Provided actionable insights for stakeholders

## Author
**Ankit Raj Sharma**  
MS Data Analytics Engineering, Northeastern University  
sharma.ankitr@northeastern.edu | [LinkedIn](#) | [Portfolio](#)

**Leadership:** Yi Chapter Head (IIIT Pune) | Community Library Founder

---

**Keywords**: Vendor Analysis, Inventory Management, Profit Optimization, Business Intelligence, Supply Chain Analytics

## Project Structure
```
├── data/                          # CSV source files
├── logs/                          # Application logs
├── inventory.db                   # SQLite database (15M+ records)
├── ingestion_db.py               # Data ingestion script
├── get_vendor_summary.py         # Data processing & aggregation
├── Exploratory Data Analysis.ipynb # Analysis notebook
├── vendor_sales_summary.csv      # Output (10,692 records)
└── README.md
```

## Key Findings

### 1. Vendor Concentration Risk
- Top 10 vendors = **65.69%** of purchases (DIAGEO 16.3%, MARTIGNETTI 8.3%, PERNOD 7.8%)
- Remaining 116 vendors = 34.31%

### 2. Bulk Purchasing Savings
| Order Size | Unit Cost | Savings |
|------------|-----------|---------|
| Small | $39.06 | - |
| Medium | $15.49 | 60% |
| Large | $10.78 | **72%** |

### 3. Inventory Optimization
- **$2.71M** in unsold capital (178 products with zero sales)
- Low turnover vendors: ALISA CARR (0.615), HIGHLAND WINE (0.708)

### 4. Profit Analysis
- Top vendors: 31.17% margin (focus on volume)
- Low vendors: 41.55% margin (focus on reach)
- **198 brands** with >65% margin need promotion

### 5. Top Performers
**Leading Brands:** Jack Daniels ($8.20M), Tito's Vodka ($7.48M), Grey Goose ($7.28M)

## Usage Examples

**Top Profitable Vendors:**
```python
import sqlite3
import pandas as pd

conn = sqlite3.connect('inventory.db')
df = pd.read_sql_query("""
    SELECT VendorName, SUM(GrossProfit) as Profit
    FROM vendor_sales_summary
    WHERE TotalSalesQuantity > 0
    GROUP BY VendorName
    ORDER BY Profit DESC LIMIT 10
""", conn)
```

**Promotional Opportunities:**
```python
df = pd.read_sql_query("""
    SELECT Brand, Description, TotalSalesDollars, ProfitMargin
    FROM vendor_sales_summary
    WHERE TotalSalesDollars < 1000 AND ProfitMargin > 65
    ORDER BY ProfitMargin DESC
""", conn)  # Returns 198 brands
```

## Business Metrics

- **Gross Profit** = Total Sales - Total Purchases
- **Profit Margin** = (Gross Profit / Total Sales) × 100
- **Stock Turnover** = Sales Quantity / Purchase Quantity
- **Sales to Purchase Ratio** = Sales Dollars / Purchase Dollars

## Recommendations

1. **Promote 198 high-margin brands** to boost volume
2. **Diversify vendor base** to reduce 65.69% concentration
3. **Leverage bulk pricing** for 72% cost savings
4. **Clear $2.71M unsold inventory** through sales/adjustments
5. **Optimize low-turnover vendors** marketing and distribution

## Technologies
Python • Pandas • SQLite • SQLAlchemy • Jupyter • Power BI • Statistical Analysis 

## Author
**Ankit Raj Sharma**  
MS Data Analytics Engineering, Northeastern University  
sharma.ankitr@northeastern.edu

---

**Keywords**: Vendor Analysis, Inventory Management, Profit Optimization, Business Intelligence, Supply Chain Analytics
