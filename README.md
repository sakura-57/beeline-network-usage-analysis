# Beeline Case — Telecom KPI Analysis

Exploratory data analysis of Beeline mobile network usage across regions of Kazakhstan, comparing key performance indicators (KPIs) for **August** and **September 2015**.

The project ingests two monthly subscriber datasets, cleans and aggregates them by market (city/region), and visualizes revenue, traffic, and usage patterns — including geographic maps of Kazakhstan, regional comparisons, and subscriber tenure analysis.

> *Originally completed in summer 2017.*

---

## Overview

| | |
|---|---|
| **Domain** | Telecommunications / Business Analytics |
| **Goal** | Compare network-usage KPIs across Kazakh regions and across two consecutive months |
| **Language** | R (100%) |
| **Period analyzed** | August – September 2015 |
| **Granularity** | Per market (`MARKET_KEY`), per month |

---

## Key Metrics (KPIs)

The analysis focuses on five core indicators per region:

- **REVENUE_TOTAL** — total revenue
- **REVENUE_VOICE** — revenue from voice services
- **REVENUE_DATA** — revenue from data services
- **TRAFFIC_DATA** — volume of data traffic
- **MOU** — Minutes of Usage (voice consumption)

Subscriber tenure is also derived from `SUBS_ACTIVATION_DATE_KEY` to study when customers joined.

---

## Repository Structure

| File | Description |
|------|-------------|
| `BeelineTask1.R` | Main R script — data loading, cleaning, aggregation, and all visualizations |
| `beelinetask1` | Supporting R code / script file |
| `Beelinecase.pptx` | Presentation of the case study and findings |
| `README.md` | Project documentation |

> **Note:** Raw input files (`August'2015.csv`, `September'2015.csv`) are not included in the repository.

---

## Workflow

### 1. Data Loading
Reads the August and September 2015 CSV exports into R.

### 2. Data Overview
Inspects structure and summary statistics with `summary()` and `str()`.

### 3. Data Preprocessing
- Aggregates KPIs by `MARKET_KEY` using **SQL queries** (`sqldf`).
- Maps short market codes to full city names, e.g.:
  - `AST → ASTANA`, `KZT → ALMATY`, `SHM → SHYMKENT`, `KAR → KARAGANDY`, `AKT → AKTAU`, and others (14 regions total).
- Merges both months into a single dataset (`full_data.csv`) with a `MONTH` label for side-by-side comparison.
- Adds a manual **population** reference table per region for context.

### 4. Analysis & Visualization
- **Geo charts** (`googleVis`): total revenue plotted on a map of Kazakhstan (`region = "KZ"`, province resolution) for each month.
- **Population table**: regions ranked by population, merged alongside the geo charts.
- **Line & point charts**: total revenue and data revenue per city, month over month.
- **Bar charts**: total / mean revenue per city, grouped by month (dodged bars).
- **Scatter plots**: Data Traffic vs. Data Revenue, with labeled regions (`ggrepel`).
- **Voice analysis**: sorted horizontal bar charts of voice revenue and voice usage (MOU) per region.
- **Box plots**: distribution of total revenue by month.
- **Tenure analysis**: frequency of subscriber activation year ("client since…").

---

## Tech Stack

**Language:** R

**Core libraries:**

| Purpose | Packages |
|---------|----------|
| Data wrangling | `dplyr`, `magrittr`, `readr`, `stringr`, `gdata`, `sqldf` |
| Visualization | `ggplot2`, `googleVis`, `ggrepel`, `gridExtra`, `grid` |
| Maps | `maps`, `ggmap` |
| Theming & color | `ggthemes`, `RColorBrewer`, `wesanderson`, `scales` |
| Dev tools | `devtools` |

---

## How to Run

1. Install R (and optionally RStudio).
2. Install the required packages:
   ```r
   install.packages(c(
     "gdata", "dplyr", "readr", "magrittr", "stringr", "sqldf",
     "googleVis", "devtools", "ggplot2", "maps", "ggmap", "scales",
     "gridExtra", "ggthemes", "RColorBrewer", "ggrepel", "wesanderson"
   ))
   ```
3. Place the two monthly CSV files where the script expects them, and update the file paths in `BeelineTask1.R` to match your local environment.
4. Run the script (e.g. `source("BeelineTask1.R")`) or step through it interactively to generate the plots and the merged `full_data.csv`.

---

## Notes & Possible Improvements

- File paths in the script are **hard-coded** to a local machine — parameterize them (or use `here::here()`) for portability.
- Input CSVs are not in the repo, so the analysis isn't reproducible as-is without the source data.
- The August and September preprocessing blocks duplicate the same city-renaming logic — this could be refactored into a reusable function.
- Region population values are entered manually inline; an external lookup file would be cleaner and easier to maintain.
