# NYC Child Care Desert Optimization

**IEOR 4004 — Project I**

This project identifies child care deserts across NYC zip codes and formulates a mixed-integer optimization model to eliminate them at minimum cost.

## Problem Overview

A zip code is classified as a **child care desert** if it fails either of two capacity thresholds:

- **Total capacity check** — total slots fall below a demand-adjusted fraction of children aged 2 weeks–12 years (50% for high-demand zips, 33% for normal-demand)
- **NYC Under-5 Policy** — slots for children aged 0–5 fall below 2/3 of the under-5 population

Demand classification (High vs. Normal) is based on average household income and employment rate.

## Optimization Model

The problem is modelled in two parts. Both tasks share the same two levers — expand existing facilities or build new ones — and minimize total cost: expansion cost + construction cost + equipment surcharge ($100/slot for every 0–5 slot added).

### Task 1 — Simplified Model

A baseline MILP with an uncapped, two-tier expansion cost structure and no spatial constraints on new facility placement.

### Task 2 — Realistic Model

Extends Task 1 with two additional constraints that reflect real-world limitations:

- **Piecewise non-continuous expansion costs** — facilities are capped at 20% capacity growth; the entire expansion is priced at a higher per-slot rate as it crosses each tier boundary (10%, 15%, 20% of current capacity)
- **Minimum distance constraint** — new facilities must be at least 0.06 miles apart (Haversine distance), preventing unrealistic clustering

## Results

| Component | Task 1 | Task 2 |
|---|---|---|
| Expansion | $62,116,283 | $6,715,728 |
| Construction | $70,175,000 (608 large, 2 medium, 1 small) | $190,030,000 (1,636 large, 11 medium, 13 small) |
| Equipment surcharge | $34,633,700 | $34,633,700 |
| **Total** | **$166,924,983** | **$231,379,428** |

## Repository Structure

```
├── Task 1/
│   ├── EDA.ipynb                        # Exploratory data analysis
│   └── Optimization_modelling.ipynb     # Desert identification + optimization model
├── Task 2/
│   ├── EDA-2.ipynb
│   └── Optimization_modelling_2_v2.ipynb
├── data/
│   ├── raw_data/                        # Source datasets (population, facilities, income, employment)
│   ├── task_1/                          # Preprocessed data for Task 1
│   ├── task_2/                          # Preprocessed data for Task 2
│   └── results/                         # Optimization outputs (summary, per-zip, per-facility)
├── Report/
│   └── Final_Report_Group29.pdf         # Final written report
├── IEOR4004_ProjectI_Description.pdf
└── IEOR4004_ProjectI_FactSheet.pdf
```

## Data Sources

- `population_nyc.csv` — population by age group and zip code
- `child_care_regulated_nyc.csv` — NYC regulated child care facilities with capacity by age group
- `avg_individual_income_nyc.csv` — average household income by zip code
- `employment_rate_nyc.csv` — employment rate by zip code
- `potential_locations_nyc.csv` — candidate locations for new facility construction

## Dependencies

- Python 3.x
- `pandas`, `numpy`
- `gurobipy` (Gurobi solver — academic license required)
