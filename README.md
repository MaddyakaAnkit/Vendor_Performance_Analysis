# Vendor Performance Data Analysis

![Vendor Performance Dashboard](dashboard_preview.png)

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

## Installation

```bash
# Setup
git clone <repo-url>
cd vendor-performance-analysis
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install pandas sqlalchemy numpy jupyter matplotlib seaborn scipy

# Run pipeline
mkdir logs data
python ingestion_db.py      # Load data (~5-10 min)
python get_vendor_summary.py # Process (~2 min)
jupyter notebook "Exploratory Data Analysis.ipynb"
```

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
sharma.ankitr@northeastern.edu | [LinkedIn](#) | [Portfolio](#)

**Leadership:** Yi Chapter Head (IIIT Pune) | Community Library Founder

---

**Keywords**: Vendor Analysis, Inventory Management, Profit Optimization, Business Intelligence, Supply Chain Analytics
