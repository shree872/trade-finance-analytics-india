# DAX Measures — Complete Reference

> All calculated measures used in the Trade Finance Analytics Power BI report.
> Each measure includes the formula, the table it belongs to, and a plain-English explanation.

---

## Table: `Factoring Turnover by Country`

### Total Turnover

```dax
Total Turnover =
SUM('Factoring Turnover by Country i'[Total Factoring Volume (Millions EUR)])
```

**Purpose:** Base aggregation measure. Sums all factoring volume values within the current filter context (respects year slicers, page filters, etc.).
**Used in:** KPI card (Total Turnover = 93.13K), line chart, donut chart.

---

### Previous Year Value

```dax
Previous Year Value =
CALCULATE(
    SUM('Factoring Turnover by Country i'[Total Factoring Volume (Millions EUR)]),
    PREVIOUSYEAR('Factoring Turnover by Country i'[Year])
)
```

**Purpose:** Shifts the filter context back by 12 months to retrieve the prior year's total.
**Why CALCULATE:** CALCULATE is the only DAX function that can modify filter context. Without it, PREVIOUSYEAR() would have no effect.
**Why PREVIOUSYEAR() not just [Year]-1:** PREVIOUSYEAR() is a time intelligence function that correctly handles edge cases (e.g., first year of data returning BLANK instead of an error).
**Prerequisite:** The `[Year]` column must be of data type Whole Number or Date for time intelligence to work correctly.

---

### Growth Rate

```dax
Growth Rate =
DIVIDE(
    [Total Turnover] - [Previous Year Value],
    [Previous Year Value]
)
```

**Purpose:** Calculates the year-over-year percentage change in factoring volume.
**Formula logic:** (Current Year − Prior Year) ÷ Prior Year = % change.
**Why DIVIDE() not /:** DIVIDE(numerator, denominator) returns BLANK when the denominator is zero or blank, preventing #DIV/0! errors. The `/` operator throws an error instead.
**Formatting:** Set to Percentage format in the model (Home → Format → Percentage).
**Used in:** Growth Rate line chart, Growth Rate KPI card (displays 4.36).

---

### Latest Turnover

```dax
Latest Turnover =
CALCULATE(
    [Total Turnover],
    LASTDATE('Factoring Turnover by Country i'[Year])
)
```

**Purpose:** Returns the turnover value for the most recent year in the dataset (2024 = 38.20K).
**Used in:** KPI card "Latest Turnover."

---

### Average Turnover

```dax
Average Turnover =
AVERAGEX(
    VALUES('Factoring Turnover by Country i'[Year]),
    [Total Turnover]
)
```

**Purpose:** Calculates the average annual turnover across all years.
**Why AVERAGEX not AVERAGE:** AVERAGEX iterates over a table (one row per year) and evaluates `[Total Turnover]` for each, giving a true per-year average. AVERAGE would average the raw column values directly, which may differ when there are multiple rows per year.
**Used in:** Gauge visual (centre value = 13.30K, max = 26.61K).

---

## Table: `Total Factoring Volume by Count`

### Total Volume

```dax
Total Volume =
SUM('Total Factoring Volume by Count'[Factoring Volume (Million EUR)])
```

**Purpose:** Aggregates factoring volume from the second table (mirrors Total Turnover but from a different source table).
**Used in:** Supporting bar chart visualisation.

---

## Formatting Applied

| Measure | Format |
|---------|--------|
| Total Turnover | Decimal Number, 2 decimal places |
| Previous Year Value | Decimal Number, 2 decimal places |
| Growth Rate | Percentage, 2 decimal places |
| Latest Turnover | Decimal Number, 2 decimal places |
| Average Turnover | Decimal Number, 2 decimal places |
| Total Volume | Decimal Number, 2 decimal places |

---

## DAX Best Practices Applied

- Used `DIVIDE()` over `/` for all division operations
- Measures reference other measures (`[Total Turnover]`, `[Previous Year Value]`) rather than repeating SUM logic — DRY principle
- All measures are stored in the model, not as calculated columns, to ensure correct filter context behaviour
- Growth Rate returns `BLANK()` for the first year (2018) where no prior year exists — this is correct behaviour
