# NYC Yellow Taxi (2022) — Risk & Strategy Analyst DIVE README

## Scope & Data
- **Dataset:** `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2022`
- **Timeframe:** Jan 1 – Nov 30, 2022 (per notebook)
- **Role Lens:** Risk & Strategy (competitive threats, exposure, scenarios)
- **Columns used:** pickup/dropoff timestamps, passenger_count, trip_distance, rate_code, store_and_fwd_flag, payment_type, fare/tax/tip/tolls/imp_surcharge/airport_fee/total_amount, pickup/dropoff_location_id, data_file_year/month

## Method (brief)
- **Pre‑cleaning CTE** reused across queries (sanity filters: passengers 1–6, 0<distance<100, total_amount>0, dropoff>pickup).
- **BigQuery** for aggregation; **pandas** for shaping; **matplotlib** for quick plots.
- **Validation stats:** t‑tests, chi‑square, ANOVA; HHI for concentration; Pearson/Spearman correlations.

---

## Data Hygiene Snapshot
**Uncleaned vs. Cleaned (headline metrics)**

| Metric | Uncleaned | Cleaned |
|---|---:|---:|
| Trips | 36,256,539 | **33,674,872** |
| Avg passengers | 1.399 | **1.430** |
| Avg distance (mi) | 6.078 | **3.524** |
| Avg fare ($) | 14.649 | **14.447** |
| Avg tip ($) | 2.720 | **2.698** |
| Avg total ($) | 21.401 | **21.256** |
| Total distance (mi) | — | **118,682,007** |
| Total fare ($) | — | **486,516,958** |
| Total tips ($) | — | **90,839,792** |
| Total amount ($) | — | **715,783,687** |

**Impact of cleaning (directional):** ~7% rows removed; long‑tail distances trimmed (upper quartiles collapse), totals down (fares −8.4%, tips −7.9%, distance −46%), removal of negative/illogical values.

---

## Discover — Patterns with Risk/Strategy Relevance

### 1) Payment Mix (cash pockets)
**Distribution (cleaned):**

| payment_type | Trips | Share |
|---:|---:|---:|
| 1 = Credit card | 26,734,534 | **79.39%** |
| 2 = Cash | 6,787,823 | **20.16%** |
| 3 = No charge | 87,206 | 0.26% |
| 4 = Dispute | 65,308 | 0.19% |
| 5 = Unknown | 1 | ~0% |

**Cash share by hour×zone (top cells)**

| Hour | Zone | Cash Share | Trips |
|---:|---:|---:|---:|
| 5 | 193 | **0.734** | 218 |
| 1 | 129 | 0.695 | 220 |
| 0 | 129 | 0.694 | 219 |
| 6 | 260 | 0.652 | 353 |
| 7 | 260 | 0.651 | 375 |
| 3 | 129 | 0.624 | 202 |
| 12 | 179 | 0.621 | 314 |
| 11 | 193 | 0.606 | 396 |
| 10 | 193 | 0.605 | 392 |
| 2 | 244 | 0.603 | 237 |

**Why it matters:** concentrated cash pockets = higher fraud/visibility risk + tip yield drag vs. cards.

### 2) SDHF — Short‑Distance, High‑Fare spikes
- Rule (D phase): **trip_distance < 1 mi AND fare_amount > $50**
- Flagged rows: **50,936** trips
- Concentration (sample cohort slice): zone **132**, rate_code **5.0**, hour **18**, month **10** → **n=27**, p80_fare ≈ **$75**, avg_fare ≈ **$71.50**

**Interpretation:** potential fixed‑fare pockets, policy edges, or mis‑encoded trips → reputational/pricing risk.

### 3) Slow‑for‑Distance windows (duration anomalies)
- Rule (D phase): duration **> 25 min/mi × distance**
- Flagged rows: **122,886** trips
- Example top cohorts (avg minutes per mile): hour 17 / zone 132 / vendor 2 / rate 5.0 → **675,501** mpm (artifact‑heavy but useful for ranking); similar spikes across zones 75, 211, 166, 239.

**Interpretation:** flags congestion/routing problem windows; metric scale is distorted by outliers → use for **relative targeting**, not literal SLAs.

### 4) Airport mix & exposure
**Month×Hour blocks (top airport gross share):**

| Month | Hour | Airport Trips | All Trips | Airport Gross Share |
|---:|---:|---:|---:|---:|
| 8 | 23 | 16,096 | 116,783 | **0.3063** |
| 1 | 6 | 3,800 | 32,660 | **0.3062** |
| 8 | 0 | 11,223 | 81,845 | 0.2993 |
| 11 | 23 | 15,758 | 119,974 | 0.2928 |
| 4 | 6 | 5,650 | 48,575 | 0.2884 |

**Airport adjacency by pickup zone (top shares):**

| Zone | Airport Trips | All Trips | Airport Share |
|---:|---:|---:|---:|
| 138 | 901,766 | 949,735 | **0.9495** |
| 132 | 1,561,388 | 1,650,814 | **0.9458** |
| 70 | 38,310 | 132,924 | 0.2882 |
| 264 | 107,104 | 390,337 | 0.2744 |
| 10 | 2,589 | 10,796 | 0.2398 |

### 5) Demand concentration (HHI)
- HHI by hour across zones (higher = more concentrated).
- Top HHI hours: **03:00, 05:00, 06:00, 02:00**; lowest: **13:00, 20:00, 18:00, 19:00**.

---

## Investigate — Deep Dives

### Payment hotspots
- Built **dfPaymentHourZone** and **dfCashHotspots**; top 5% cash_share cells have **trips > 200** and cash_share up to **0.73**.
- Visual: Top Cash‑Share Hotspots (hour×zone) used for targeting.

### SDHF concentration
- 50,936 short‑distance high‑fare trips; segmented by **month/dow/hour/rate_code/zone**.
- Many spikes tied to **rate_code 5.0** and specific zones (e.g., 132, 129, 230) during evening hours.

### Slow‑for‑Distance cohorts
- Ranked **hour×zone×vendor×rate_code** by **avg minutes per mile**; top cohorts exceed 95th‑pct threshold massively (artifact‑sensitive → use for ranking only).

### Airport exposure
- Month×hour blocks with **airport_gross_share ~27–31%**; zone‑level airport adjacency ~0.95 (zones **138, 132**).

### Concentration windows
- Constructed **dfHourZone** and per‑hour **HHI**; highest concentration in early morning hours.

---

## Validate — Statistical Tests & Persistence

### Cash hotspots are statistically distinct & persistent
- **95th pct threshold (cash_share):** **0.3295**
- **t‑test (hotspots vs baseline cash_share):** t ≈ **83.66**, p ≈ **0.000**
- **Persistence (examples):**

| Hour | Zone | Months Observed | Months ≥ Baseline |
|---:|---:|---:|---:|
| 2 | 132 | 11 | **11** |
| 1 | 132 | 11 | **11** |
| 0 | 75 | 11 | **11** |
| 3 | 230 | 11 | **11** |
| 3 | 132 | 11 | **11** |

### SDHF clustering is non‑random
- **Threshold (sdhf_rate 95th pct):** **1.0000** (due to p99 cut within short trips; many small cohorts become 100%)
- **Chi‑square (zone vs any SDHF):** χ² ≈ **818.36**, p ≈ **3.55e‑113**
- **Persistence (examples):**

| Zone | Rate | Months | Months Hot | Avg sdhf_rate |
|---:|---:|---:|---:|---:|
| 238 | 2.0 | 10 | 9 | **0.9972** |
| 233 | 2.0 | 8 | 7 | 0.9967 |
| 100 | 2.0 | 8 | 6 | 0.9953 |
| 132 | 3.0 | 9 | 6 | 0.9950 |
| 239 | 2.0 | 9 | 6 | 0.9933 |

### Slow‑for‑Distance cohorts differ materially
- **95th pct threshold (min_per_mile):** **584.65**
- **ANOVA (hotspots vs baseline):** F ≈ **367.50**, p ≈ **5.64e‑70**
- **Consistency (examples):**

| Hour | Zone | Vendor | Rate | Months | Months Hot | Mean mpm |
|---:|---:|---:|---:|---:|---:|---:|
| 13 | 170 | 2 | 1.0 | 7 | 2 | **4,559.30** |
| 17 | 237 | 2 | 1.0 | 7 | 2 | 563.15 |
| 12 | 236 | 2 | 1.0 | 6 | 2 | 490.74 |
| 18 | 237 | 2 | 1.0 | 6 | 2 | 420.67 |
| 15 | 236 | 2 | 1.0 | 9 | 2 | 419.04 |

### Airport dependence ↔ revenue volatility
- **Correlations (zone‑level):** Pearson ≈ **0.175**; Spearman ≈ **0.402** (positive).
- **Illustrative zones:**

| Zone | Rev Mean ($) | Rev Std ($) | Volatility | Airport Share |
|---:|---:|---:|---:|---:|
| 138 | 3,953,687 | 716,341 | **0.181** | **0.9498** |
| 132 | 9,205,969 | 1,686,115 | **0.183** | **0.9458** |
| 70 | 539,503 | 82,746 | 0.153 | 0.2856 |
| 264 | 1,085,910 | 286,756 | 0.264 | 0.2592 |
| 10 | 67,604 | 20,060 | 0.297 | 0.2288 |

### Demand concentration (HHI) is meaningfully higher at night
- **Top vs bottom hours:** ΔHHI ≈ **0.0159**; t ≈ **31.15**; p ≪ **0.001**

---

## Extend — Experiments & Scenario Models (with outputs)

### 1) Cash→Card shift in validated hotspots
- **Setup:** top‑N cash hotspots (N=50); shift **20%** of cash trips to card.
- **Baseline in hotspot windows:** cash trips **382,588**; card trips **944,732**; avg_fare ≈ **$44.82**.
- **Tip gap:** card **26.6556%** vs cash **0.0028%** → gap **≈ 26.6528%**.
- **Estimated incremental tips:** **$913,990** (≈ **1.01%** of 2022 total tips).

### 2) SDHF audit guardrails
- **Top zone/rate pairs by SDHF volume** (examples): (132, 2.0), (170, 2.0), (230, 2.0), (161, 2.0), (264, 2.0), (129, 5.0)…
- **Monthly trend example (zone 129, rate 5.0):** Jan **16/17** (94.1%), Feb **26/27** (96.3%), Mar **51/54** (94.4%), … Oct **126/135** (93.3%), Nov **120/126** (95.2%).

### 3) Slow‑for‑Distance mitigation windows
- **Top cohorts by minutes_saved_per_trip** (10% reduction proxy; artifact‑heavy scale):

| Hour | Zone | Vendor | Rate | Trips | Avg Miles | MPM | Minutes Saved/Trip |
|---:|---:|---:|---:|---:|---:|---:|---:|
| 15 | 186 | 2 | 1.0 | 40 | 2.44 | 213,363.85 | **52,061.41** |
| 17 | 162 | 2 | 1.0 | 41 | 2.05 | 204,877.70 | 41,993.67 |
| 14 | 236 | 2 | 1.0 | 36 | 1.88 | 203,879.46 | 38,294.40 |
| 13 | 100 | 2 | 1.0 | 46 | 2.19 | 188,645.06 | 41,239.74 |
| 11 | 246 | 2 | 1.0 | 22 | 2.29 | 188,527.95 | 43,184.18 |

> **Use as ranking**, not literal minutes. Cap/robustify in production.

### 4) Airport dependency buffer (−10% shock)
- **Top exposed zones (illustrative):**

| Zone | Airport Share | Airport Gross ($) | Total Gross ($) | Shock ΔGross (−10%) |
|---:|---:|---:|---:|---:|
| 138 | **0.9495** | 41,336,063.94 | 43,490,692.89 | **−4,133,606.39** |
| 132 | **0.9458** | 95,166,225.37 | 101,266,413.70 | **−9,516,622.54** |
| 70 | 0.2882 | 1,762,137.78 | 5,934,733.73 | −176,213.78 |
| 264 | 0.2744 | 5,263,052.08 | 11,945,032.12 | −526,305.21 |
| 10 | 0.2398 | 189,782.32 | 743,645.16 | −18,978.23 |

### 5) Concentration windows → coverage rebalancing
- Use top‑HHI hours (03:00–06:00) to pre‑position supply; pair with cash/airport overlays to avoid compounding exposures.

---

## Strategic Implications (exec‑ready)
- **De‑risk acceptance:** Targeted cash→card in a few hour×zone pockets yields ~**$0.9M** in incremental tips (proxy KPI) and reduces risk.
- **Tighten fare governance:** SDHF cohorts (esp. specific zone/rate combos) need guardrails + monthly audit.
- **Buffer airport reliance:** High airport‑share zones show higher volatility; pre‑plan staffing/marketing for demand shocks.
- **Improve reliability:** Route guidance in slow‑for‑distance windows (robustified metric) to cut predictable delays.
- **Balance coverage:** Address high‑HHI hours via pre‑positioning and intra‑borough balancing.

---

## Limitations
- Internal‑only lens; no external competitor/pricing data.
- Duration metric is outlier‑sensitive (use ranks/robust caps for ops).
- Zone IDs shown without external names (by scope choice).