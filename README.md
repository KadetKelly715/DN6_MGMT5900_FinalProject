# Team README — Strategic Analytics & AI-Driven Insights (NYC Yellow Taxi 2022)

## Overview
Consulting-style DIVE analysis (Discover → Investigate → Validate → Extend) on the 2022 NYC Yellow Taxi dataset to develop executive-ready strategy for competing against rideshare platforms. Each teammate completed an individual DIVE in their specialty; the team synthesized findings into a dashboard and a short video.

## Live Assets
- **Interactive Dashboard (Looker Studio):** https://lookerstudio.google.com/reporting/d1a3c4a9-644c-4baf-91c0-6793d1a01dc4  
- **Video Walkthrough (YouTube, 3–5 min):** https://youtu.be/J2TzdpNXB50

## Repository Structure
DN6_59000_FinalProject/
├── Individual_Analyses/
│ ├── Financial_Analyst/
│ │ ├── DIVE_Analysis.ipynb
│ │ └── README.md
│ ├── Customer_Analyst/
│ │ ├── DIVE_Analysis.ipynb
│ │ └── README.md
│ ├── Operations_Analyst/
│ │ ├── DIVE_Analysis.ipynb
│ │ └── README.md
│ ├── Risk_Strategy_Analyst/
│ │ ├── DIVE_Analysis.ipynb
│ │ └── README.md
│ └── Tech_Innovation_Analyst/ (if applicable)
│ ├── DIVE_Analysis.ipynb
│ └── README.md
├── Team_Deliverables/
│ ├── Executive_Strategy_Presentation.pdf
│ └── Team_Video_Link.txt
├── Supporting_Materials/
│ ├── Interactive_Dashboard/ (Looker screenshots or notebook for alt dashboard)
│ └── Collaboration_Log.md
└── README.md ← (this file)


## Business Challenge
A NYC taxi operator is losing share to rideshare competitors. The team’s goal is to identify strategic levers—pricing, product, operations, partnerships, and risk posture—to stabilize core demand and grow profitable segments.

## Data
- **Primary table:** `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2022`
- **Scope:** Yellow taxi trips in 2022 (public BigQuery dataset)
- **Key fields used:** timestamps, passenger_count, trip_distance, payment_type, itemized fares (fare/tip/tolls), surcharges, pickup/dropoff location IDs, and totals.

> All queries run directly in BigQuery; notebooks are in Colab with BigQuery Python client.

## Methods (DIVE Framework)
- **Discover:** Baseline distributions, volume and revenue patterns, payment mix, geospatial hotspots.
- **Investigate:** Drivers of profitability and demand (time-of-day, zones, airport flows, cash vs. card, trip length).
- **Validate:** Outlier handling, alternative explanations, sensitivity checks, and cross-metric consistency.
- **Extend:** Scenario tests (e.g., airport dependency shocks, cash→card shifts), and prioritized recommendations with expected impact.

## Roles & Contributions
- **Financial Performance:** Revenue, margins, and fare/tip structure; unit economics by segment.
- **Customer/Market:** Segment behavior, demand pockets, and competitive positioning.
- **Operational Excellence:** Throughput, reliability, bottlenecks, and service quality levers.
- **Risk & Strategy:** Scenario planning, threat/opportunity mapping, and portfolio of strategic options.
- **Technology & Innovation (if used):** Digital capability gaps and ROI of tech investments.

## How to Reproduce (Colab + BigQuery)
1. Open the relevant `DIVE_Analysis.ipynb` in Colab.
2. Authenticate to Google Cloud and initialize the BigQuery client.
3. Set your project ID in the notebook.
4. Run cells in order. Queries reference the public table above.

**Example query pattern (Colab + BigQuery Python client):**
```python
query = f"""
SELECT
  payment_type,
  COUNT(*) AS num_trips,
  ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2022`), 2) AS pct
FROM `bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2022`
GROUP BY 1
ORDER BY 2 DESC;
"""
df = bq_client.query(query).to_dataframe()
df.head()
