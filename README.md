
# US County Emissions Dashboard — Built with Databricks & PySpark

![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=databricks&logoColor=white)
![Delta Lake](https://img.shields.io/badge/Delta%20Lake-00ADD8?style=for-the-badge&logo=delta&logoColor=white)

*An end-to-end pipeline (ingestion → Spark transformations → SQL analysis → dashboard) across 3,142 US counties*


A Databricks end-to-end project exploring greenhouse gas emissions across ~3,142 US counties — from raw data to an interactive, cross-filterable dashboard.

## Overview
Built entirely in Databricks (Unity Catalog, PySpark, SQL, Lakeview Dashboards), this project takes a raw county-level energy dataset through cleaning, transformation, and analysis, ending in a dashboard answering four core questions about where — and why — emissions concentrate across the US. The initial 4-chart concept was inspired by a tutorial video; this version corrects an aggregation bug in the original approach, adds a climate/grid-mix analysis not in the original plan, and offers alternative chart types (Pareto, 100% stacked bar) for comparison.

## Dataset
- **Source:** NREL/DOE County Energy Profiles — county-level electricity, natural gas, population, employment, and GHG emissions data
- **Grain:** 1 row per US county (~3,142 rows), 29 columns after cleanup
- **Location:** [`data/`](./data)

## Tech stack
Databricks · Unity Catalog · Delta Lake · PySpark · SQL · Lakeview (AI/BI) Dashboards

## Pipeline
1. Raw CSV landed in a Unity Catalog Volume
2. Loaded & explored in a PySpark notebook — schema check, null audit, dropped fully-empty columns
3. Cleaned & transformed — renamed columns to a consistent `<sector>_<metric>_<unit>` convention, fixed columns that loaded as strings instead of numeric types, derived `emission_per_person`
4. Saved as a managed Delta table
5. Explored further in the SQL Editor — temp tables, aggregations, CTEs, window functions
6. Built a multi-chart dashboard with shared, cross-filtering fields

## Business questions
- **Q1** — Where is emissions physically concentrated, and does it track population?
- **Q2** — Does population size predict a county's emissions?
- **Q3** — Which states emit disproportionately more (or less) than their population share?
- **Q4** — How concentrated is national emissions among the top-emitting counties?
- **Bonus** — Does climate, or electricity-grid composition, better explain per-capita emissions?

## Key findings
- Population alone doesn't predict emissions — Harris County, TX and Cook County, IL out-emit Los Angeles County despite far smaller populations.
- California holds ~20% of the US population but only ~5.6% of modeled emissions; Florida and Georgia both over-emit relative to population — a possible climate effect.
- Top per-capita states cluster around historically coal-heavy grids (MO, TN, WV, KY) — suggesting grid composition, not just climate, drives per-capita emissions.

Full write-up: [`US_Emissions_Dashboard_Insights.md`](./US_Emissions_Dashboard_Insights.md)

## Repo structure
```
├── notebooks/    # Load & explore, transform & clean
├── queries/      # Saved SQL queries (aggregation, CTEs, window functions)
├── dashboard/    # Dashboard export (.lvdash.json, PDF, screenshots)
├── data/         # Raw dataset + source note
├── US_Emissions_Dashboard_Insights.md
└── README.md
