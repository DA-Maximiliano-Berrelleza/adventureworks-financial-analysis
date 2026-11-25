# AdventureWorks | International Financial Performance Analysis with SQL

[Español](README.md) | English

I analyzed AdventureWorks' 2017 sales across six markets to compare revenue, costs, gross profit, margin, and ROI. The objective was to provide evidence to help prioritize markets and inform marketing budget decisions.

I combined sales, products, categories, and territories using SQL, calculated country-level metrics, and incorporated campaign spending. I presented the results in Excel and developed recommendations using a Context → Finding → Implication structure.

**Context:** Academic project completed during a data analytics bootcamp.  
**Tools:** SQL and Excel. The queries use type conversions such as `::integer` and `::numeric`.

## Business questions

- How much gross profit does each country generate?
- How do markets compare when gross profit is measured against campaign spending?

## Data and workflow

The sales data contains one row per product and order. I used `ventas_2017`, `productos`, `productos_categorias`, `territorios`, and `campanas`.

| Stage | Work completed | Queries |
|---|---|---|
| Exploration | Inspected the available tables. | [01_exploracion.md](01_exploracion.md) |
| Preparation | Joined tables using `LEFT JOIN`, handled missing values with `COALESCE`, and calculated revenue and cost per line item. | [02_preparacion.md](02_preparacion.md) |
| Metrics | Aggregated data by country, incorporated campaign spending, and calculated gross profit, margin, and ROI, using `NULLIF` to prevent division by zero. | [03_indicadores.md](03_indicadores.md) |
| Validation | Checked for missing keys, invalid quantities, and negative prices. | [04_validaciones.md](04_validaciones.md) |

### Documented validation results

The queries found **0 missing values** in `numero_pedido`, `clave_producto`, and `clave_territorio`; **0 rows** with `cantidad_pedido <= 0`; and **0 products** with `precio_producto < 0`.

### Metrics used

| Metric | Project formula |
|---|---|
| Revenue | Sum of product price × quantity ordered. |
| Costs | Sum of product cost × quantity ordered. |
| Gross profit | Revenue − costs. |
| Margin (%) | Gross profit ÷ revenue × 100. |
| ROI (%) | Gross profit ÷ campaign cost × 100. |

## Results by country

The following table reproduces the metrics in the Excel file. Monetary amounts are shown to two decimal places and rates as percentages.

| Country | Revenue | Costs | Campaign spending | Gross profit | Margin | ROI |
|---|---:|---:|---:|---:|---:|---:|
| United States | 3,353,939.92 | 1,899,471.33 | 1,920,000.00 | 1,454,468.60 | 43.37% | 75.75% |
| Australia | 2,532,003.49 | 1,474,958.18 | 2,150,400.00 | 1,057,045.31 | 41.75% | 49.16% |
| United Kingdom | 1,189,636.78 | 681,508.55 | 2,304,000.00 | 508,128.24 | 42.71% | 22.05% |
| Germany | 1,071,460.42 | 611,295.36 | 2,265,600.00 | 460,165.06 | 42.95% | 20.31% |
| France | 924,316.93 | 527,797.14 | 2,208,000.00 | 396,519.79 | 42.90% | 17.96% |
| Canada | 710,205.18 | 392,325.91 | 1,824,000.00 | 317,879.27 | 44.76% | 17.43% |

- **The United States** generated the highest revenue and gross profit, with the highest ROI in the analysis: **75.75%**.
- **Australia** ranked second in gross profit at **1,057,045.31**, with an ROI of **49.16%**.
- **Canada** had the highest margin, **44.76%**, alongside the lowest revenue and ROI, **17.43%**.
- **The United Kingdom, Germany, and France** had margins between **42.71% and 42.95%**, with ROI between **17.96% and 22.05%**.

## Recommendations

1. **Gradually increase advertising investment in the United States.** Since this market has the highest ROI in the analysis, run a pilot with a **10–20%** budget increase. Monitor ROI and gross profit before expanding investment to assess whether the increase maintains the efficiency observed.

2. **Audit the advertising strategy in Europe and Canada.** Review targeting, marketing channels, and customer acquisition cost in the United Kingdom, Germany, France, and Canada, given their lower ROI compared with the United States and Australia. This review would help identify opportunities for improvement and inform campaign and budget adjustments.

## Results files

- [Excel file with metrics, executive summary, and project responses](Resultados_AdventureWorks.xlsx).
- [Metrics table in CSV format](Resultados_AdventureWorks.csv).

The CSV contains only the six-country table. `Margen_pct` and `ROI_pct` retain the decimal scale used in Excel: for example, `0.7575` represents `75.75%`.

## Query sequence and dependencies

The TripleTen environment saved intermediate results for use in later stages:

| Intermediate table | Result represented | Where it is used |
|---|---|---|
| `ventas_clean` | Second preparation query: integrated detail with revenue and cost per line item. | First and second metrics queries. |
| `pais_ingreso_costo` | First metrics query: revenue and costs by country and territory. | Final metrics query. |
| `pais_campanas` | Second metrics query: revenue, costs, and campaign spending by territory. | Final metrics query. |

The Markdown files present the queries in SQL code blocks and preserve the `SELECT` statements documented in the course. Running them outside that environment requires the source and intermediate tables with equivalent structures and data. This repository contains queries and exported results; the course database is not included.

## Source documentation

- [Project document: instructions, queries, results, and evaluation](https://docs.google.com/document/d/1kMlZ2qaPa2eE2hIZ4oMiO0PjxVw9AVRjn5KsbQ94yzE/edit).
- [Results file and executive summary](https://docs.google.com/spreadsheets/d/18KTDt9F5ICos_CMCmpIs4y6qaCVom6Rh/edit).
