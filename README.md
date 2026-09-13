# Impact of the COVID-19 Pandemic on Market Sectors (2018–2023)

**Language:** English | [Deutsch](README_DE.md)

This portfolio project analyzes how four market sectors — **Technology, Pharmaceuticals, Travel, and Entertainment** — developed before, during, and after the COVID-19 pandemic.

The project originated as a capstone project in a six-month Data Analytics Bootcamp. The original Python analysis was my main contribution. For this portfolio version, I cleaned and extended the notebook, corrected the sector methodology to an **equal-weighted approach**, added risk analysis, and built a new interactive **Power BI dashboard** based on the same star-schema data model.

## Key Findings

- **Technology** achieved the strongest equal-weighted full-period sector return at 322.37%, ahead of **Pharmaceuticals** at 166.06%.
- **Entertainment** returned 43.10% and **Travel** 11.41%.
- **Travel** was the only sector with a negative average return during the COVID period (-6.40%) and also had the deepest COVID drawdown (-62.05%).
- **Pharmaceuticals** combined solid returns with the lowest annualized volatility of the four sectors (19.09%).
- At company level, **NVIDIA** (904.24%), **Eli Lilly** (664.92%), and **Microsoft** (368.71%) were the strongest performers; **Carnival** (-69.82%) and **Warner Bros. Discovery** (-50.76%) the weakest.

## Project Overview

| Item | Scope |
|---|---|
| Sectors | 4 |
| Assets | 24 stocks and ETFs, 6 per sector |
| Analysis period | January 2018 – December 2023 |
| Trading days | 1,509 |
| Price observations | 36,216 |
| Data model | Star Schema with one fact table and two dimension tables |
| Main analysis | Python / Jupyter Notebook |
| Interactive extension | Power BI |
| Main tools | Python, pandas, Matplotlib, yfinance, Jupyter, Power BI, DAX |

## Research Questions

The analysis focuses on four questions:

1. How did the four sectors perform over the full 2018–2023 period?
2. Which individual companies had the strongest and weakest performance?
3. How did sector returns differ between the **Pre-COVID**, **COVID**, and **Post-COVID** periods?
4. How did risk differ across sectors and individual assets, measured by annualized volatility and maximum drawdown?

## Data and Methodology

Historical market data is downloaded from Yahoo Finance with `yfinance` for the period from **2018-01-01** to **2024-01-01** (exclusive end date), covering the full 2018–2023 analysis period.

The notebook uses `auto_adjust=True`, so adjusted OHLC prices are used consistently.

Each sector contains six assets. Technology, Pharmaceuticals, and Travel include one sector ETF; Entertainment contains six individual companies.

### Sector construction

For the main sector performance analysis, the project uses an **equal-weighted methodology**:

1. Each asset is normalized individually to a starting value of 100.
2. The normalized asset series are averaged within each sector.
3. Sector total return is calculated from the average of the individual asset returns.

This avoids giving higher-priced shares more influence simply because their absolute share price is larger.

### Period definition

The six-year period is divided into three two-year phases:

- **Pre-COVID:** 2018–2019
- **COVID:** 2020–2021
- **Post-COVID:** 2022–2023

This is an analytical simplification rather than a statistically identified structural break.

### Return calculation

For an individual asset or period:

`(Ending Value / Starting Value − 1) × 100`

### Risk analysis

Annualized volatility is calculated as:

`Standard Deviation of Daily Returns × √252`

Maximum drawdown measures the largest decline from a previous cumulative peak.

---

# Python / Jupyter Analysis

The Jupyter Notebook is the **main analytical part of the project**. It contains the full workflow from market-data download and cleaning to exploratory analysis, risk analysis, interpretation, and export of the dimensional data model.

## 1. Equal-Weighted Sector Performance Index

![Equal-Weighted Sector Performance Index](images/sector_performance_index.png)

All assets are first normalized individually to 100 and then averaged within their sector.

Technology shows the strongest long-term development, while Travel experiences the most visible disruption during the 2020 market shock.

## 2. Equal-Weighted Total Return by Sector

![Equal-Weighted Total Return by Sector](images/total_return_by_sector.png)

| Sector | Total Return |
|---|---:|
| Technology | 322.37% |
| Pharmaceuticals | 166.06% |
| Entertainment | 43.10% |
| Travel | 11.41% |

Technology clearly outperformed the other sectors over the full period.

## 3. Returns Before, During, and After COVID

![Sector Return by COVID Period](images/sector_return_by_period.png)

This is the central comparison for the project.

| Sector | Pre-COVID | COVID | Post-COVID |
|---|---:|---:|---:|
| Technology | 54.32% | 125.83% | 14.08% |
| Pharmaceuticals | 27.86% | 64.72% | 12.98% |
| Entertainment | 37.55% | 31.29% | -26.18% |
| Travel | 1.62% | -6.40% | 6.74% |

Technology and Pharmaceuticals performed strongly during the COVID period. Travel was the only sector with a negative average return during that phase. Entertainment weakened clearly in the Post-COVID period.

## 4. Annualized Volatility by Sector

![Annualized Volatility by Sector](images/annualized_volatility_by_sector.png)

| Sector | Annualized Volatility |
|---|---:|
| Pharmaceuticals | 19.09% |
| Entertainment | 26.99% |
| Technology | 29.97% |
| Travel | 39.17% |

Travel had the highest annualized volatility, while Pharmaceuticals showed the most stable price behavior of the four selected sectors.

## 5. Risk vs. Return by Asset

![Risk vs. Return by Asset](images/risk_vs_return_by_asset.png)

The scatter plot compares each asset's full-period return with its annualized volatility.

- **NVIDIA** achieved the strongest return at 904.24%.
- **Eli Lilly** returned 664.92%.
- **Microsoft** returned 368.71%.
- High volatility did not automatically lead to high returns: **Carnival** and **Warner Bros. Discovery** combined comparatively high risk with negative full-period performance.

## 6. Maximum Drawdown During COVID

![Maximum Drawdown During COVID](images/maximum_drawdown_covid.png)

| Sector | Maximum Drawdown |
|---|---:|
| Travel | -62.05% |
| Entertainment | -33.13% |
| Technology | -30.45% |
| Pharmaceuticals | -26.43% |

Travel experienced by far the deepest drawdown during the COVID period, while Pharmaceuticals showed the smallest decline.

## Individual Company Performance

The notebook also ranks individual assets across the full period.

Strongest performers include:

- NVIDIA: 904.24%
- Eli Lilly: 664.92%
- Microsoft: 368.71%

Weakest performers include:

- Carnival: -69.82%
- Warner Bros. Discovery: -50.76%
- U.S. Global Jets ETF: -40.93%

These results should not be interpreted as effects of COVID alone. Sector trends, interest rates, inflation, technological developments, company-specific events, and other market factors also influenced performance.

---

# Star Schema

The cleaned market data is transformed into a dimensional model for downstream analysis.

![Power BI Star Schema](images/star_schema_power_bi.png)

| Table | Role | Main fields |
|---|---|---|
| `fact_prices` | Fact table | `AssetID`, `DateID`, Open, High, Low, Close, Volume |
| `dim_assets` | Asset dimension | `AssetID`, Company, Sector, Ticker, Currency |
| `dim_date` | Date dimension | `DateID`, Date, Year, Quarter, Month, Weekday, Period |

Relationships:

- `dim_assets (1) → fact_prices (*)`
- `dim_date (1) → fact_prices (*)`

The `Period` field assigns each trading day to the Pre-COVID, COVID, or Post-COVID phase. It is defined once in the date dimension, so the period logic is identical in Python and Power BI.

The Power BI model also contains a dedicated `_Measures` table for DAX measures.

## Data Quality

The notebook checks the dataset before analysis.

- **24 assets**
- **1,509 trading days**
- **36,216 observations**
- **0 missing values**
- complete `AssetID` and `DateID` relationships
- no duplicate asset/date combinations in the exported fact table

---

# Interactive Power BI Dashboard

After completing the Python analysis, I built an additional Power BI report using the exported star-schema tables. Power BI is an **interactive extension of the Python analysis**, not a replacement for it.

The report contains three pages.

## Page 1 — Sector Overview

![Sector Overview](images/sector_overview_power_bi.png)

This page provides an interactive overview of sector performance with:

- Base-100 equal-weighted sector performance
- total return by sector
- sector and date filters
- dataset summary cards

## Page 2 — Company Performance & Risk

![Company Performance and Risk](images/company_performance_risk_power_bi.png)

This page moves from sector level to individual assets:

- full-period company return ranking
- Risk vs. Return scatter plot
- company, sector, and date filters

## Page 3 — Pandemic Impact & Key Findings

![Pandemic Impact and Key Findings](images/pandemic_impact_power_bi.png)

This page directly addresses the pandemic comparison:

- Pre-COVID, COVID, and Post-COVID sector returns
- maximum drawdown during 2020–2021
- concise key findings

The Power BI report uses DAX measures for equal-weighted returns, Base-100 performance, annualized volatility, period returns, and COVID drawdown.

---

# Limitations

- Absolute share prices are not directly comparable across companies, which is why normalized values and percentage returns are used for the main comparisons.
- Three sectors contain one ETF, while Entertainment contains six individual companies, so the sector compositions are not perfectly equivalent.
- The Pre-COVID, COVID, and Post-COVID periods are predefined analytical categories rather than statistically identified structural breaks.
- The observed market developments cannot be attributed to the pandemic alone.
- The analysis covers 24 selected assets and is not intended to represent the entire market.
- The assets were selected retrospectively rather than through random sampling, so selection bias may affect the results.
- `dim_date` contains trading days only and is not a continuous calendar table for standard DAX time-intelligence functions.
- A fresh Yahoo Finance download may reflect later corporate actions or adjusted-price changes, so exact values can differ from the exported project dataset.

# Files

- `covid_sector_analysis.ipynb` — complete Python/Jupyter analysis
- `data/fact_prices.csv` — fact table, 36,216 rows
- `data/dim_assets.csv` — asset dimension, 24 rows
- `data/dim_date.csv` — date dimension, 1,509 rows
- `covid_sector_analysis.pbix` — interactive Power BI report
- `images/` — notebook and Power BI visualizations

# Running the Project

The exported CSV files in `data/` allow the project to be reviewed without downloading the market data again.

```bash
pip install pandas matplotlib yfinance jupyter
jupyter notebook covid_sector_analysis.ipynb
```

A full notebook run downloads market data again from Yahoo Finance and overwrites the files in `data/`.

## Conventions

Code comments, chart labels, and the main README are written in English so the project can be reviewed internationally.

---

This project is a Data Analytics bootcamp capstone that was cleaned, extended, and developed further as a portfolio project. It does not constitute investment advice.
