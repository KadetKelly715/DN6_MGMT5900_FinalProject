# Individual DIVE Analysis — Technology & Innovation Analyst (NYC Taxi 2022)

**Author:** Prasanti  
**Notebook:** `DIVE_Tech_Innovation_2022_NY_Taxi_Prasanti.ipynb`  
**Role:** Technology & Innovation Analyst (DIVE Part 1)

## What this is
A reproducible DIVE analysis using the NYC TLC 2022 **yellow+green** trips to evaluate digital payment adoption, connectivity (SAF), and ROI proxies.

## Dataset & tools
- BigQuery tables:  
  - `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2022`  
  - `bigquery-public-data.new_york_taxi_trips.tlc_green_trips_2022`  
  - `bigquery-public-data.new_york_taxi_trips.taxi_zone_geom`
- Join: cast `PULocationID`/`pu_zone_id` → `zone_id`, left join to `taxi_zone_geom` to derive `pu_borough`
- Environment: Google Colab + BigQuery

## How to run
1) Setup/Bootstrap (auth + BigQuery helper)  
2) Auto-fix schema + zone join (sets `join_col`, `boro_col`)  
3) Build canonical trips CTE (`canonical_trips_sql`)  
4) Run sections in order: **Discover → Investigate → Validate (simple OLS + ROI proxy) → Extend**  
   - If you hit a type mismatch on the join, re-run Auto-fix then re-run the canonical SQL cell.

## Key results
- Targeting cutoffs: **low digital ≤ 0.466**, **high SAF ≥ 0.012**  
- Top boroughs to start: **Brooklyn, Bronx, EWR**  
- ROI proxy (+10pp digital in low-adoption segments): **$257,939**

## DIVE summary
- **Discover:** borough×month KPIs (digital, SAF, tips, trips)  
- **Investigate:** low-digital + high-SAF gaps; digital share ↗ tip rate  
- **Validate:** simple OLS slope for digital → dollars; +10pp scenario to dollars  
- **Extend:** payments UX + connectivity + nudges; pilot in top boroughs

## KPIs & guardrails
Digital share ↑, tip rate ↑, SAF share ↓, failures ↓, disputes ↓; **staged rollout with monitoring.**

## Limitations
Correlation ≠ causation; would add covariates or experiments for causal claims.

## Files
- Notebook (`.ipynb`) with outputs  
- `README.md` (this file)

