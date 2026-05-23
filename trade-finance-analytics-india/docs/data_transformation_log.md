# Data Transformation Log

> Step-by-step record of every transformation applied in Power Query (M Language) during the project.
> This document serves as an audit trail and reproducibility guide.

---

## File 1: FIMIS_DATA.xlsx

### Sheet: `Total Factoring Volume by Count`

| Step # | Action | Power Query Step Name | Detail |
|--------|--------|-----------------------|--------|
| 1 | Load sheet | Source | Connected via Get Data → Excel Workbook |
| 2 | Promote headers | Promoted Headers | First row set as column headers |
| 3 | Set data types | Changed Type | Year → Whole Number; Factoring Volume → Decimal Number |
| 4 | Rename table | (in model) | Renamed to `Total Factoring Volume by Count` |

---

### Sheet: `Factoring Turnover by Country`

| Step # | Action | Power Query Step Name | Detail |
|--------|--------|-----------------------|--------|
| 1 | Load sheet | Source | Connected via Get Data → Excel Workbook |
| 2 | Promote headers | Promoted Headers | First row set as column headers |
| 3 | Set data types | Changed Type | Year → Whole Number; Turnover → Decimal Number |
| 4 | Rename table | (in model) | Renamed to `Factoring Turnover by Country i` |

---

### Sheet: `Sectoral Deployment of Non-Food Credit` ⚠️ Major Transformation

**Problem:** Data arrived in wide format — each year was a separate column (date-stamped column headers like `2019-03-29`, `2020-03-27`, etc.). Power BI cannot perform time-series analysis on wide-format data.

**Solution:** Unpivot all year columns into a single `Year` column with a corresponding `Credit Amount` value column.

| Step # | Action | Power Query Step Name | Detail |
|--------|--------|-----------------------|--------|
| 1 | Load sheet | Source | Get Data → Excel Workbook |
| 2 | Promote headers | Promoted Headers | First row → column headers |
| 3 | Remove top junk rows | Removed Top Rows | Removed 2 blank/aggregate header rows |
| 4 | Select year columns | (manual select) | Selected all date-stamped year columns |
| 5 | **Unpivot columns** | **Unpivoted Columns** | Transform → Unpivot Columns → wide → long |
| 6 | Rename `Attribute` | Renamed Columns | `Attribute` → `Year` |
| 7 | Rename `Value` | Renamed Columns | `Value` → `Credit Amount` |
| 8 | Extract year integer | Extracted Year | Used Text.Start([Year], 4) to get YYYY from date string |
| 9 | Set data types | Changed Type | Year → Whole Number; Credit Amount → Decimal Number |
| 10 | Remove nulls | Filtered Rows | Removed rows where Sector = null (header artifact rows) |

**Resulting schema:**
```
| Sector (Text) | Year (Integer) | Credit Amount (Decimal) |
```

---

## File 2: india_forfaiting_exports.xlsx

### Sheet: `Sheet1`

| Step # | Action | Power Query Step Name | Detail |
|--------|--------|-----------------------|--------|
| 1 | Load sheet | Source | Get Data → Excel Workbook |
| 2 | Promote headers | Promoted Headers | First row → column headers |
| 3 | Set data types | Changed Type | Year → Whole Number; all numeric columns → Decimal |
| 4 | Handle null growth % | (base year) | 2015 rows have null growth — expected (no prior year), left as BLANK |
| 5 | Rename table | (in model) | Used as `India Forfaiting Export Data` |

---

## Data Modeling Steps (Post Power Query)

| Step | Action | Detail |
|------|--------|--------|
| 1 | Create relationship | Linked `Factoring Turnover[Year]` ↔ `Total Factoring Volume[Year]` on Year key |
| 2 | Verify cardinality | Confirmed One-to-One relationship on shared Year dimension |
| 3 | Set cross-filter direction | Single direction (standard for this model) |
| 4 | Hide foreign key columns | Hid `Year` in subordinate tables to keep field list clean |

---

## Assumptions Documented

| Assumption | Justification |
|------------|---------------|
| EUR values are nominal, not inflation-adjusted | No deflator data available in source files |
| Sectoral dates treated as annual snapshots | Last reporting Friday of March = year-end proxy |
| Null/`-` values treated as missing, not zero | Treating blanks as zero would distort averages and growth rates |
| Forfaiting volumes are estimates | FCI data does not capture informal transactions |
