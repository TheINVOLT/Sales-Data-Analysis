[Uploading README.md…]()
# Sales-Data-Analysis
End-to-end Python EDA of 50,000 sales orders (2010-2017): finds which products drive profit vs. revenue, tests region/channel effects, and models pricing and mix-shift opportunities.
# Data Analyst Assessment: Sales Profit Mix Analysis

**Dataset:** 50000 Sales Records (Excel BI Analytics) · **Approach:** Option A, Python (pandas, NumPy, SciPy, Matplotlib, Seaborn) · **Dashboard:** Google Looker Studio

> **Read this first.** The publisher of this dataset states that it is *generated through random logic in VBA* and is *not real sales data* (source page below). It was chosen for its size and its price-cost-profit structure. Every finding in this submission describes **this dataset**, not a real company or market. Recommendations are framed as hypotheses to confirm on real data.

## What is in this folder

| File | What it is |
|---|---|
| `Sales_Analysis_Assessment.xlsx` | The single workbook. Import it into Google Sheets (File > Import > Upload, "Replace spreadsheet"). Tabs: `Data` (raw, 50,000 rows), `Processed Data` (cleaned, 27 columns), `Q1` to `Q10`, plus `Analysis Tables` (live formulas) and `Charts`. |
| `sales_analysis.ipynb` | Jupyter / Google Colab notebook with the full analysis (cleaning, tests, scenarios, charts). |
| `sales_analysis.py` | The same code as a plain script: `python sales_analysis.py 50000_Sales_Records.csv output` |
| `Sales_Profit_Mix_Management_Deck` | 7-slide management presentation (download as .pptx/PDF or upload to Google Slides). |
| `looker_dashboard_guide.md` | Step-by-step build of the Looker Studio dashboard from the `Processed Data` tab. |
| `charts/` | PNG charts produced by the code. |
| `README.md` | This file. |

## Source

- Page: https://excelbianalytics.com/downloads-18-sample-csv-files-data-sets-for-testing-sales/
- File: https://excelbianalytics.com/wp/wp-content/uploads/2017/07/50000-Sales-Records.zip
- Order priority codes (from the publisher): H = High, M = Medium, L = Low, C = Critical.

## Business problem

Revenue is about $8.7B a year at a 29.5% margin and has been flat (+0.3% a year, 2010 to 2016). Where does profit really come from, and which levers could raise it without relying on volume growth?

## Method

1. **Profile**: shape, types, missing values, duplicates, duplicate Order IDs.
2. **Validate**: re-compute Revenue = Units x Price, Cost = Units x Unit Cost, Profit = Revenue - Cost (all match within $0.02); no order ships before it is ordered.
3. **Clean and standardise**: text trimmed; US `m/d/yyyy` text dates converted to real dates with an explicit format; priority codes given readable labels.
4. **Calculated fields** (13 added): Order Year / Month / Quarter, Year-Month, Days To Ship, Profit Margin, Unit Profit, Partial Year Flag, Margin Tier, Order Size Band, Profit Band, Revenue Outlier Flag, Priority Label.
5. **Outliers**: orders above the IQR cut-off ($4.1M revenue; 3,453 orders) are **flagged, not removed**; they are valid and carry a large share of profit.
6. **Partial year**: the data ends 28 Jul 2017, so 2017 is flagged and growth is measured on 2010 to 2016 only.
7. **Analysis**: segment tables, trend and seasonality, Welch t-tests and one-way ANOVA, eta-squared variance decomposition, and what-if arithmetic for price and mix levers.
8. **Annualising**: the dataset spans 7.6 years, so what-if dollar figures are shown as whole-period totals and as per-year equivalents.

## Headline results (this dataset)

- Product category explains 100% of margin differences; region, channel and priority explain under 0.01%.
- Office Supplies and Meat are 34% of revenue but 19.5% of profit.
- Cosmetics (18.6% of profit at 39.8% margin) and Clothes (67.2% margin) are under-weighted relative to profit.
- Revenue is flat; the apparent 44% fall in 2017 is a seven-month artefact.
- Critical orders ship in 25.0 days on average, the same as Low (24.9).

## Known limitations

See sheet `Q6`. In short: synthetic data; one fixed price and cost per category (so price response cannot be measured); uniform random order sizes; no customer, discount or return data; scenario figures are arithmetic upper bounds, not forecasts.

## How to reproduce

```
pip install pandas numpy scipy matplotlib seaborn openpyxl
python sales_analysis.py 50000_Sales_Records.csv output
```

Outputs: `output/processed_data.csv`, `output/results.json`, `output/tables.xlsx`, `output/charts/*.png`.

## AI use

Claude (Anthropic) was used for planning, code, drafting and the deck. See sheet `Q10` for what it was used for and an example of a result that had to be checked and corrected.
