# Delhivery_FeatureEngineering
An exploratory and in-depth study of the commerce logistics data for  Delhivery. Using Concepts: Feature Creation,,normalisation,standardisation,outliers treatment etc.

## About Delhivery:
Delhivery is the largest and fastest-growing fully integrated player in India by revenue in Fiscal 2021. They aim to build the operating system for commerce, through a combination of world-class infrastructure, logistics operations of the highest quality, and cutting-edge engineering and technology capabilities.
The Data team builds intelligence and capabilities using this data that helps them to widen the gap between the quality, efficiency, and profitability of their business versus their competitors.

## Column Profiling:

data - tells whether the data is testing or training data
trip_creation_time – Timestamp of trip creation
route_schedule_uuid – Unique Id for a particular route schedule
route_type – Transportation type
FTL – Full Truck Load: FTL shipments get to the destination sooner, as the truck is making no other pickups or drop-offs along the way
Carting: Handling system consisting of small vehicles (carts)
trip_uuid - Unique ID given to a particular trip (A trip may include different source and destination centers)
source_center - Source ID of trip origin
source_name - Source Name of trip origin
destination_cente – Destination ID
destination_name – Destination Name
od_start_time – Trip start time
od_end_time – Trip end time
start_scan_to_end_scan – Time taken to deliver from source to destination
is_cutoff – Unknown field
cutoff_factor – Unknown field
cutoff_timestamp – Unknown field
actual_distance_to_destination – Distance in Kms between source and destination warehouse
actual_time – Actual time taken to complete the delivery (Cumulative)
osrm_time – An open-source routing engine time calculator which computes the shortest path between points in a given map (Includes usual traffic, distance through major and minor roads) and gives the time (Cumulative)
osrm_distance – An open-source routing engine which computes the shortest path between points in a given map (Includes usual traffic, distance through major and minor roads) (Cumulative)
factor – Unknown field
segment_actual_time – This is a segment time. Time taken by the subset of the package delivery
segment_osrm_time – This is the OSRM segment time. Time taken by the subset of the package delivery
segment_osrm_distance – This is the OSRM distance. Distance covered by subset of the package delivery
segment_factor – Unknown field

## Concept Used:

-Feature Creation

-Relationship between Features

-Column Normalization /Column Standardization

-Handling categorical values

-Missing values - Outlier treatment / Types of outliers

## Methodology:
1. Basic data cleaning and exploration:
- Handle missing values in the data.
- Analyze the structure of the data.
- Try merging the rows using the hint mentioned above.
2. Build some features to prepare the data for actual analysis. Extract features from the below fields:
-Destination Name: Split and extract features out of destination. City-place-code (State)
-Source Name: Split and extract features out of destination. City-place-code (State)
-Trip_creation_time: Extract features like month, year and day etc
3. In-depth analysis and feature engineering:
-Calculate the time taken between od_start_time and od_end_time and keep it as a feature. Drop the original columns, if required
-Compare the difference between Point a. and start_scan_to_end_scan. Do hypothesis testing/ Visual analysis to check.
-hypothesis testing/ visual analysis between actual_time aggregated value and OSRM time aggregated value (aggregated values are the values you’ll get after merging the rows on the basis of trip_uuid)
-hypothesis testing/ visual analysis between actual_time aggregated value and segment actual time aggregated value (aggregated values are the values you’ll get after merging the rows on the basis of trip_uuid)
-hypothesis testing/ visual analysis between osrm distance aggregated value and segment osrm distance aggregated value (aggregated values are the values you’ll get after merging the rows on the basis of trip_uuid)
-hypothesis testing/ visual analysis between osrm time aggregated value and segment osrm time aggregated value (aggregated values are the values you’ll get after merging the rows on the basis of trip_uuid)
-Find outliers in the numerical variables (you might find outliers in almost all the variables), and check it using visual analysis
-Handle the outliers using the IQR method.
-one-hot encoding of categorical variables (like route_type)
-Normalize/ Standardize the numerical features using MinMaxScaler or StandardScaler.

## Recommendations
There is a significant difference between OSRM and actual parameters.
There is a need to:

Revisit information fed to routing engine for trip planning. Check for discrepancies with transporters, if the routing engine is configured for optimum results.

North,South and West zones corridors have significant traffic of orders. But, we have a smaller presence in Central,Eastern and North-Eastern zone. However, it would be difficult to conculde this, by looking at just 2 months data. It is worth investigating and increasing our presence in these regions.

From state point of view, we have heavy traffic in Maharastra followed by Karnataka. This is a good indicator that we need to plan for the resources on ground in these 2 states on priority. Especially during festive seasons.

🚚 Routing & Delivery Optimization
1. Address Right-Skewed Trip Durations
- Action: Flag long-tail trips (>2000 units) for root cause analysis.
- Why: These outliers may reflect inefficiencies, delays, or routing anomalies.
- Tool: Use percentile-based thresholds to auto-flag top 5% longest trips.
2. Segment-Level Bottleneck Detection
- Action: Investigate segments with unusually high segment_actual_time or segment_factor.
- Why: These may indicate traffic congestion, poor connectivity, or failed attempts.
- Tool: Build a dashboard tile showing segment-level outliers by hub or route type.

🧠 Predictive Model Calibration
3. OSRM Time Underestimation
- Action: Apply correction factors to OSRM time estimates, especially for long-haul FTL routes.
- Why: Scatterplots show consistent underprediction for longer trips.
- Tool: Use regression residuals to build a bias-adjusted prediction model.
4. Distance Prediction Reliability
- Action: Continue using OSRM for distance-based planning.
- Why: Strong linear correlation and low dispersion validate its accuracy.
- Tool: Integrate OSRM distance into cost modeling and fuel estimation workflows.

🏢 Hub-Level Strategy
5. Gurgaon_Bilaspur_HB Dominance
- Action: Prioritize infrastructure, fleet, and manpower at this hub.
- Why: It handles ~23,000 trips—far exceeding others.
- Tool: Build a hub performance dashboard with volume, delay, and SLA metrics.
6. Tiered Hub Classification
- Action: Classify hubs into High, Mid, and Low volume tiers.
- Why: Enables tailored resource allocation and SLA expectations.
- Tool: Use clustering on trip volume and delay metrics.

📈 Temporal Demand Planning
7. Peak Hour Load Balancing
- Action: Reinforce operations during 10 PM and 7–9 PM windows.
- Why: These hours show highest trip counts.
- Tool: Dynamic shift scheduling and vehicle dispatch planning.
8. Midday Lull Optimization
- Action: Use 10 AM–3 PM window for maintenance, training, or low-priority deliveries.
- Why: Lowest trip volumes occur here.
- Tool: Time-slot based task allocation.

📍 Geographic Strategy
9. State-Level Prioritization
- Action: Focus on Haryana, Maharashtra, Karnataka, Tamil Nadu, Gujarat for scaling.
- Why: These states dominate both source and destination volumes.
- Tool: Geo-mapping dashboards with volume overlays and SLA heatmaps.
10. Tail-End State Engagement
- Action: Explore growth strategies in Himachal Pradesh, Uttarakhand, Goa.
- Why: Low volumes may reflect untapped demand or delivery constraints.
- Tool: Pilot programs, alternate delivery models, or regional partnerships.

📊 Cutoff Logic & Route Type Insights
11. FTL Cutoff Enforcement
- Action: Review cutoff logic for FTL routes with high enforcement (~85,000 cases).
- Why: May impact SLA compliance and cost.
- Tool: Build a cutoff impact simulator to test delivery outcomes with/without cutoff.
12. Carting Route Efficiency
- Action: Leverage Carting routes for short-haul, high-frequency deliveries.
- Why: More consistent and predictable performance.
- Tool: Route-type based delivery planning and SLA tuning.

---

## 👨‍💻 Author  
**Swati Singh**  

📧 [swati.autodidact@gmail.com](mailto:swati.autodidact@gmail.com)  
🔗 [LinkedIn Profile](https://www.linkedin.com/in/swatisinghlink/)  

---

