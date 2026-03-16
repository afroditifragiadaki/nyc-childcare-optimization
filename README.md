# NYC Child Care Desert Optimization

**IEOR 4004 — Project I**

This project identifies child care deserts across NYC zip codes and formulates a mixed-integer optimization model to eliminate them at minimum cost.

## Problem Overview

A zip code is classified as a **child care desert** if it fails either of two capacity thresholds:

- **Total capacity check** — total slots fall below a demand-adjusted fraction of children aged 2 weeks–12 years (50% for high-demand zips, 33% for normal-demand)
- **NYC Under-5 Policy** — slots for children aged 0–5 fall below 2/3 of the under-5 population

Demand classification (High vs. Normal) is based on average household income and employment rate.

## Optimization Model

The model decides, for each of the 179 desert zip codes, how to allocate new capacity at minimum cost by choosing:

- **Expand existing facilities** — add 0–5 or 5–12 slots to existing regulated facilities (two-tier cost structure based on expansion size relative to current capacity)
- **Build new facilities** — construct new Small (100 slots), Medium (200 slots), or Large (400 slots) facilities

The objective minimizes total cost: expansion cost + construction cost + equipment surcharge ($100/slot for every 0–5 slot added).

**Result:** Minimum cost to eliminate all 179 deserts — **$166,924,983**

| Component | Cost |
|---|---|
| Expansion | $62,116,283 |
| Construction (608 large, 2 medium, 1 small) | $70,175,000 |
| Equipment surcharge | $34,633,700 |

## Repository Structure

```
├── Task 1/
│   ├── EDA.ipynb                    # Exploratory data analysis
│   └── Optimization_modelling.ipynb # Desert identification + optimization model
├── Task 2/
│   ├── EDA-2.ipynb
│   └── Optimization_modelling_2.ipynb
├── data/
│   ├── raw_data/                    # Source datasets (population, facilities, income, employment)
│   ├── task_1/                      # Preprocessed data for Task 1
│   ├── task_2/                      # Preprocessed data for Task 2
│   └── results/                     # Optimization outputs (summary, per-zip, per-facility)
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
