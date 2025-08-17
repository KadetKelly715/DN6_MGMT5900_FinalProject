# **NYC Taxi Trips**
# **Customer and Market Analyst DIVE**

##**Challenge:** Traditional Taxi Business Transformation
##**Client:** New York City taxi company losing market share
##**Dataset:** All taxi trips completed in NYC in 2022
##**Question:** How should we restructure our operations and service model to compete more effectively with on-demand ridesharing platforms?
 


##**Dataset:** bigquery-public-data.new_york_taxi_trips.tlc_yellow_trips_2022
##**Link:** https://console.cloud.google.com/marketplace/product/city-of-new-york/nyc-tlc-trips?hl=en&inv=1&invt=Ab5q2w&project=pep-workforcemanagement-prod
##**Location zone id .csv file:** https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

#**Data Clean-up:**

###To ensure that our analysis is accurate and reliable, we removed inconsistencies and outliers from the data and made sure it was filtered to the year 2022 only. Below we demonstrate the change before and after cleaning the data. There is a significant decrease of ~3 million trips.

| Metric | Uncleaned | Cleaned |
|---|---:|---:|
| Total trips | 36,256,539 | **33,669,476** |
| Avg passengers | 1.40 | **1.43** |
| Avg distance | 6.08 | **3.52** |
| Avg fare | 14.65 | **14.43** |
| Avg tip  | 2.72 | **2.70** |
| Avg total amount  | 21.401 | **21.23** |
| Total distance  | 220,355,289.21 | **118,647,726.74** |
| Total fare  | 531,128,142.56 | **485,710,843.81** |
| Total tips  | 98,600,948.34 | **90,819,898.63** |
| Total amount  | 775,931,967.69 | **714,941,694.54** |


#DISCOVER – Identify Customer Segments, Patterns and Dynamics

##  1. Identify customer segments, patterns, and dynamics. 



| No. | trip_segment | total_trips | avg_trip_distance | avg_fare_amount | avg_passenger_count |	avg_trip_duration_minutes |
|---:|---:|---:|---:|---:|---:|---:|
| 0 | Late-Night Traveler  | 6306180  | 2.840000000  | 11.850000000  | 1.48  | 13.83  |
| 1 | Other  | 6037074  | 2.190000000  | 10.450000000  | 1.90  | 12.75  |
| 2 | Evening Commuter  | 5548256  | 2.050000000  | 10.660000000  | 1.16  | 13.97  |
| 3 | Errand Runner  | 4042954  | 1.140000000  | 7.960000000  | 1.37  | 10.56  |
| 4 | Morning Commuter  | 3911056  | 2.050000000  | 10.510000000  | 1.11  | 13.18  |
| 5 | Airport Traveler  | 3378707  | 13.900000000  | 42.270000000  | 1.51  | 39.21  |
| 6 | Social Traveler  | 2171629  | 3.800000000  | 15.780000000  | 1.21  | 20.03  |
| 7 | Casual Shopper  | 2080314  |3.290000000  | 15.820000000  | 1.41  | 22.82  |
| 8 | Business Traveler  | 139263  | 7.090000000  | 28.450000000  | 1.61  | 36.33  |
| 9 | Tourist  | 54043  | 7.760000000  | 27.670000000  | 3.93  | 31.82  |

### We use the data to identify the key characteristics of each segment. The goal is to find patterns that suggest a trip's purpose. Gemini helped us locate 10 different customer segments: Late-Night Traveler, Other, Evening Commuter, Errand Runner, Morning Commuter, Airport Traveler, Social Traveler, Casual Shopper, and Business Traveler. The results show a detailed nuanced view of the customer base.


###**Which pickup/drop-off zones show highest and lowest demand?**
| No.  | demand_type  | location_id  | number_of_trips  |
|---:|---:|---:|---:|
| 0  | Highest Demand Dropoff  | 236  | 1462890  |
| 1  | Lowest Demand Pickup  | 84  | 5  |
| 2  | Lowest Demand Dropoff  | 110  | 3  |
| 3  | Highest Demand Pickup  | 132  | 1649959  |


###**How do trip distances, durations, and purposes vary by time and location?**

| No.  | hour_of_day  | day_of_week  | pickup_location_id  | trip_segment  | total_trips  | avg_trip_distance  | avg_trip_duration_minutes  | avg_fare_amount  |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 0  | 0  | 1  | 1  | Airport Traveler  | 1  | 0.040000000  | 0.00 | 169.000000000  |
| 1  | 0  | 1  | 10  | Tourist  | 4  | 17.540000000  | 32.50  | 80.880000000  |
| 2  | 0  | 1  | 10  | Other  | 18  | 14.800000000  | 23.67  | 55.860000000  |
| 3  | 0  | 1  | 10  | Airport Traveler  | 1  | 29.700000000  | 60.00  | 130.000000000  |
| 4  | 0  | 1  | 100  | Other  | 2777  | 2.820000000  | 16.15  | 12.330000000  |
| …  | …  | …  | …  | … | …  | …  | …  | …  |
| 61539  | 23  | 7  | 95  | Tourist  | 5  | 8.690000000  | 30.00  | 29.400000000  |
| 61540  | 23  | 7  | 96  | Other  | 1  | 0.600000000  | 3.00  | 4.500000000  |
| 61541  | 23  | 7  | 97  | Tourist  | 8  | 7.040000000  | 25.63  | 24.810000000  |
| 61542  | 23  | 7  | 97  | Other  | 225  | 4.300000000  | 17.93  | 16.640000000  |
| 61543  | 23  | 7  | 98  | Other  | 1  | 15.030000000  | 28.00  | 42.500000000  |

### **Key Observations:** This segment by segment breakdown allows us to create a clearer picture of customer behavior. We've identified five distinct customer segments: Airport Travelers are high-value customers with long trips and high fares. Late-Night Travelers are the most frequent group, providing crucial volume with their short-to-medium-distance trips. Social Travelers are active on weekends, taking moderate-fare trips for leisure. Business Travelers are a premium weekday segment, while Errand Runners & Casual Shoppers provide a steady, predictable revenue stream with their frequent, short-distance trips throughout the day. This segmentation highlights diverse revenue opportunities that our restructuring plan should target.

#**INVESTIGATE – Drivers Behavior**
## **1. Segments Decline**
### Commuters are lost on convenience: The decline in this high-volume segment isn't about trip quality but about efficiency. Rideshare apps offer a more predictable and streamlined experience, which is what time-sensitive commuters value most. The taxi service is losing the race for speed.
### High-Value Travelers are dissatisfied: The low tip percentages from Airport Travelers and Business Travelers are a significant red flag. It indicates a disconnect between the high fare they pay and their perceived value of the service. These customers are likely switching to services that offer better price transparency, seamless payment, and a more professional experience.
## **2. Unmet Demand is Found in the Niches**
### The "Other" segment is a major opportunity: This large, unclassified group represents a goldmine of potentially high-value, long-distance trips that are currently being overlooked.
### Late-night and long-distance trips are key: While commuters and errand runners provide a steady base, the Late-Night Traveler segment is a huge growth area due to its higher fares and longer trip distances. This, along with the long-distance trips in the "Other" segment, is where the most valuable unmet demand lies.
## **3. Taxis Have an Edge in Underserved Areas**
### The taxi fleet is still the go-to for long trips: In rideshare-dominated zones (referred to as "low-demand zones" in your analysis), the taxi fleet is still the preferred choice for longer, more expensive trips. This suggests that while rideshares have captured the short-trip market, your service retains a valuable niche for more complex or long-distance journeys.
### Commuters in "low-demand zones" are different: The analysis shows that commuters in these areas take longer trips and pay higher fares than those in high-demand zones, suggesting a reliance on taxis due to a lack of other convenient, affordable transportation options. This is a valuable niche to protect.

#**VALIDATE – Test Segmentation and Behavior Analysis**

##**1. Could observed trends be due to pandemic aftereffects or macroeconomic shifts?**

| Row  | data_year  | total_trips  | avg_total_amount  | avg_passenger_count  | total_trip_distance  | avg_trip_distance  | total_fare_amount  | avg_fare_amount  | total_tip_amount  | avg_tip_amount  | total_tolls_amount  | total_amount  |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
|1  | 2022  | 33669476	  | 21.234120024  | 1.4295223067920642  | 118647726.74  | 3.52389585  | 485710843.81  | 14.425850994  | 90819898.63  | 2.697395666  | 17857596.64  | 3305770.5  | 714941694.54  |
| 2  | 2019  | 81745242  | 18.958851671  | 1.5937009642714137 | 246407202.81  | 3.014330826  | 1075469775.03  | 13.156359303  | 179703149.7  | 2.198331613  | 30751498.02 | 1549795917.87  |

### Based on the analysis, the taxi business has undergone a fundamental shift from a high-volume, low-value model to a lower-volume, high-value one. This change could be driven by the pandemic's impact on commuting and increased competition from rideshare services, which have significantly reduced the number of daily commuter trips. In contrast, higher-value segments like Airport Travelers and Tourists have proven more resilient. The data supports this by showing a sharp decrease in total trips and revenue from 2019 to 2022, but a notable increase in the average fare, tip, and trip distance. To adapt, the company's new strategy should prioritize attracting and retaining these lucrative, long-distance customers.



#**EXTEND - Acquisition/Retention Strategies**
##**What targeted marketing or loyalty programs could win back high-value customers?**
###Offer a flat-rate, pre-booked fare for common commuter routes during peak hours. This removes the uncertainty of cost, a major pain point for commuters.
###Since commuters have a high card-payment percentage, lean into technology. Ensure a seamless app experience with quick booking, in-app payments, and clear route visualization.
###Due to the high number of customer in the Other segment, we need to understand them to keep customer retention. We need to do a deeper analysis and groups by time of day, day of the week and location. This could uncover new, high volume segments like late-night travelers or suburban travelers.
##**Which geographic areas should be prioritized for re-entry or expansion?**
###The data showed that airport travelers, while these are high-value customers, their tip percentage is the lowest, suggesting potential dissatisfaction. The taxi company should consider creating a loyalty program for frequent airport travelers.
###The data suggests that tourists are a high value segment. We could keep and increase tourist retention by increasing the visibility of the service and simplifying accessibility. The taxi company could offer bundle packages for tour groups and create partnerships with hotels and tourist attractions.