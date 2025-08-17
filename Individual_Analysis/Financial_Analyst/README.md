# DIVE Analysis – NYC Yellow Taxi Trips (Jan–Nov 2022)

## Author
Joon Chua

## Summary
NYC Yellow Cabs have been losing market share to ride-sharing companies. This analysis applies the DIVE method to trip data from January to November 2022 (with outliers excluded) to identify key revenue drivers and propose strategies for regaining market share.

## Methodology
The analysis followed the DIVE framework:

- **Discovery:** Defined objectives, reviewed NYC Yellow Cab trip data, and identified relevant metrics (revenue, trip counts, payment methods, vendors).  
- **Investigate:** Explored temporal patterns, geographic distribution, payment methods, and vendor performance.  
- **Validate:** Confirmed findings through comparative analysis (e.g., seasonality, event-driven demand).  
- **Extend:** Developed financial projections and strategic recommendations to compete with ride-sharing companies.  

## Data Source
The dataset was obtained from [Google Cloud Public Datasets – NYC TLC Trips](https://console.cloud.google.com/marketplace/product/city-of-new-york/nyc-tlc-trips?hl=en&inv=1&invt=Ab5uLg&project=pep-workforcemanagement-prod).  
This analysis uses Yellow Taxi trip data from January to November 2022, with outliers excluded.

---

## Discovery
The analysis reveals that financial performance, based on net revenue, is shaped by seasonal demand patterns, trip volume, trip type, and payment behavior.

### Key Findings

#### 1. Revenue Trends
- **Seasonality:** Revenue peaked in October ($79.7M) and bottomed in January ($46M). Spring and fall were high-demand periods; summer saw a dip due to fewer business travelers.
- **Trip Volume vs. Fares:** Average fare per trip stayed steady — volume, not pricing, drove peaks.
- **Time Patterns:** Weekday evenings (5–6 PM) and weekend nightlife were top revenue periods; Sundays had lower revenue with peaks at midday (12–5 PM).

#### 2. Cost Patterns & Profitability Drivers
- **Vendor Performance:** Verifone (Vendor ID 2) generated more than double CMT (Vendor ID 1), with a more profitable trip mix.
- **Rate Code Profitability:** Standard rate had the largest share (79.69%) but lowest avg. net revenue/trip ($16.24). Airport and negotiated fares were most profitable per trip.
- **Pass-Through Costs:** Highest for airport trips but still profitable due to higher fares.
- **Tips:** $97M total; 76.46% came from credit cards, could also suggest a potential gap in data for cash tips.
- **Extra Charges:** Averaged $1.00 per trip, mainly rush-hour surcharges.

#### 3. Customer Segmentation
- **High-Volume, Local Riders:** Short trips (0–2 miles) within Manhattan — lower per-trip profit but bulk of total revenue.
- **High-Profit, Airport Riders:** Less frequent but high-value trips to/from JFK and Newark.
- **Payment Behavior:** Credit cards consisted 79.07% of reported revenue; higher avg. per trip than cash.


---

## Investigate

Based on the combined findings, financial performance is primarily impacted by three key factors: trip profitability, operational efficiency, and structural business models of the vendors.

### Factors Impacting Financial Performance

#### 1. Trip Volume as Primary Revenue Driver
- Monthly revenue fluctuations mirror trip volume changes.
- High-volume months tied to business travel, good weather, and events. For example, NYC Marathon and Memorial Day generated spikes; Fourth of July and Thanksgiving caused predictable declines.
- Low winter volumes reduce revenue regardless of fare rates.

#### 2. Trip Type & Distance Economics
- Short-haul (0–2 mi) generated highest net revenue/hour ($68.47).
- Long-haul generated higher net fare/trip.
- Midtown Manhattan, Lower Manhattan zones, and high-density commercial areas have high turnover, low pass-through costs.
- Airport trips profitable even after high fees — Newark highest/trip, JFK highest total.

#### 3. Payment Method & Tipping
- Credit card payments yield higher reported tips, while cash payments show lower tips, possibly due to incomplete data.
- Card-heavy zones/times have higher net revenue/trip.

#### 4. Vendor Performance Variance
- Verifone dominates profitable zones (JFK, Newark).
- CMT focuses on short-haul, lower-value trips.
- Once normalized, driver performance is similar.

#### 5. Operational Efficiency
- High-margin zones reduce idle time.
- Peak-hour concentration boosts driver earnings/hour.

#### 6. Demand Patterns
- Standard rate dominates.
- Demand peaks: weekday evenings, weekend nights.


---

## Validate
The Validate phase has successfully confirmed several key assumptions about the financial drivers of NYC yellow cab revenue. 

### Confirmed Assumptions
#### 1. Trip Volume Dominates Gross Revenue
- Fare/trip is stable; growth must come from more trips.

#### 2. Pass-Through Fees Are Neutral
- Excluding tolls/fees gives true profitability view.

#### 3. Seasonal/Event Trends Are Predictable
- Peaks: weekday commutes, weekend nightlife.
- Seasonal dips: summer; seasonal highs: fall.
- Event boost: Certain holidays/events increase trips and fares. 

#### 4. Weekday evening rush hours generate high volume
- Driven possibly by business travel after working hours.

#### 5. High-Density Zones > Airport Zones (for sustainable profit)
- More trips/hour in urban cores vs. fewer, longer airport trips.

#### 6. Vendor Operations Matter
- Vendor strategy (trip mix, zone focus) impacts revenue.


---

## Extend: Financial Projections and Investment Recommendations  

### Financial Projections  
Based on revenue trends (Discovery), key drivers (Investigate), and validated correlations (Validate), the following revenue impact is projected if Yellow Cab adapts competitive strategies against ride-sharing companies:  

- **Baseline (no change):** Annual revenue projected to remain flat or decline at 2–3% YoY due to continued ride-sharing competition.  
- **With targeted interventions:** Revenue can grow **5–8% annually** over the next 3 years, stabilizing market share.  
- **Upside potential:** By fully capturing untapped demand from airports, evening commuters, and app-based hailing, revenues could rise **10–12% annually**.  


### Investment Recommendations  
To compete effectively, Yellow Cab should focus on the following areas:  

#### 1. Digital Competitiveness  
- **Mobile App Upgrade**: Modernize the hailing and payment app to match ride-sharing user experience (real-time tracking, upfront pricing, digital tipping, instant booking).  
- **Dynamic Pricing**: Implement flexible fare adjustments during peak events and holidays.  
- **Loyalty Program**: Discounts for frequent riders, corporate packages for business travelers.  

#### 2. Operational Efficiency  
- **Fleet Optimization**: Incentivize drivers to cover high-demand areas (airports, Midtown, nightlife zones).  
- **Data Analytics**: Real-time monitoring of demand zones to reduce idle time and increase trip density.  

#### 3. Customer Experience  
- **Cashless Default**: Promote credit card/digital payments for transparency and reliable tip reporting.  
- **Service Quality**: Driver training and customer service standards to differentiate from ride-sharing complaints.  


### Implementation Timeline  

| Phase | Initiative | Timeline | Resources Required |  
|-------|------------|----------|--------------------|  
| Phase 1: Foundation | Data analytics system, app redesign, corporate partnerships | 0–6 months | $2–3M (tech dev, partnerships, training) |  
| Phase 2: Growth | Loyalty program, marketing campaigns, dynamic pricing rollout | 6–12 months | $1–2M (marketing, incentives, IT ops) |  
| Phase 3: Expansion | Scale to all boroughs, integrate with events/airport demand | 12–24 months | $3–4M (fleet incentives, system scaling) |  


### Resource Requirements  
- **Technology Investment:** $5–7M over 2 years (app, pricing engine, analytics platform).  
- **Human Capital:** Driver training, customer service staff, marketing team.  
- **Partnerships:** Corporate travel accounts, event organizers, hospitality industry.  


### Expected ROI  
- **Payback Period**: 18–24 months.  
- **Net ROI**: 25–30% over 3 years, assuming successful app adoption and corporate ridership contracts.  
- **Breakdown**:  
  - 5–8% revenue lift from digital enhancements.  
  - 3–5% from operational efficiency gains.  
  - 2–3% from corporate/event partnerships.  


### Success Metrics  
- **Financial**: Revenue growth rate (target +8% annually), average net fare per trip, reduction in idle driver hours.  
- **Operational**: Increase in airport/business district trip share, average trip utilization per driver.  
- **Customer**: App adoption rate (target 70% of rides booked via app in 2 years), customer satisfaction scores, loyalty program participation.  


--

## Conclusion  
By upgrading digital capabilities, optimizing operations, and improving customer experience, NYC Yellow Cabs can stabilize revenues and recover market share lost to ride-sharing companies. With disciplined investment and execution, positive ROI within two years is realistic.  
