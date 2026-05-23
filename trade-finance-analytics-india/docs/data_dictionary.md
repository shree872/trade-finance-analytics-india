# Data Dictionary

> Complete column definitions for all datasets used in the Trade Finance Analytics project.

---

## 1. FIMIS_DATA.xlsx

### Sheet: `Total Factoring Volume by Count`

| Column | Data Type | Description | Unit | Source |
|--------|-----------|-------------|------|--------|
| Year | Integer | Calendar year of observation | YYYY | FIMIS Portal |
| Factoring Volume (Million EUR) | Decimal | Total factoring volume transacted in India for that year | Million EUR | RBI / FIMIS |

---

### Sheet: `Factoring Turnover by Country`

| Column | Data Type | Description | Unit | Source |
|--------|-----------|-------------|------|--------|
| Year | Integer | Calendar year of observation | YYYY | FIMIS Portal |
| Total Factoring Volume (Millions EUR) | Decimal | Aggregate factoring turnover (used for DAX measures) | Millions EUR | RBI / FIMIS |

*Note: This sheet mirrors the Volume sheet and serves as the primary table for DAX time intelligence calculations.*

---

### Sheet: `Sectoral Deployment of Non-Food Credit`

**Raw (wide) format — before transformation:**

| Column | Data Type | Description |
|--------|-----------|-------------|
| Sr. No. | Integer | Row identifier |
| Sector | Text | Economic sector name (Agriculture, MSME, Industry, etc.) |
| 2019-03-29 | Decimal | Credit deployed as of March 2019 reporting date |
| 2020-03-27 | Decimal | Credit deployed as of March 2020 reporting date |
| 2021-03-26 | Decimal | Credit deployed as of March 2021 reporting date |
| 2022-03-25 | Decimal | Credit deployed as of March 2022 reporting date |
| 2023-03-24 | Decimal | Credit deployed as of March 2023 reporting date |
| 2024-03-22 | Decimal | Credit deployed as of March 2024 reporting date |
| 2025-03-21 | Decimal | Credit deployed as of March 2025 reporting date |

**After Power Query Unpivot transformation (processed format):**

| Column | Data Type | Description | Unit |
|--------|-----------|-------------|------|
| Sector | Text | Economic sector name | — |
| Year | Integer | Year extracted from reporting date | YYYY |
| Credit Amount | Decimal | Non-food credit deployed to that sector | ₹ Crore (assumed) |

---

### Sheet: `Industry-Wise Deployment of GBC`

| Column | Data Type | Description |
|--------|-----------|-------------|
| Industry | Text | Industry category (Coal, Mining, Iron & Steel, etc.) |
| 1990–2007 | Decimal | Annual gross bank credit to that industry (yearly columns) |

*Note: This dataset covers historical 1990–2007 data. Not the primary dataset for dashboard visuals.*

---

## 2. india_forfaiting_exports.xlsx

### Sheet: `Sheet1`

| Column | Data Type | Description | Unit | Notes |
|--------|-----------|-------------|------|-------|
| Year | Integer | Calendar year | YYYY | 2015–2024 |
| Export_Value_USD_Billion | Decimal | India's total merchandise export value | USD Billion | Not inflation-adjusted |
| Forfaiting_Volume_USD_Billion | Decimal | Total forfaiting transactions volume | USD Billion | Estimated / FCI data |
| Export_Growth_% | Decimal | YoY percentage change in export value | % | Null for 2015 (base year) |
| Forfaiting_Growth_% | Decimal | YoY percentage change in forfaiting volume | % | Null for 2015 (base year) |

---

## Assumptions & Limitations

- All EUR-denominated values are **nominal** (not inflation-adjusted).
- Forfaiting volume figures are estimates compiled from FCI annual statistics and may not capture informal/unreported transactions.
- Sectoral credit dates represent the last reporting Friday of March each year — treated as the annual year-end snapshot.
- Missing values (`"-"`) in FIMIS data are treated as `null` and excluded from aggregations.
