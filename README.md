# TransLogix Freight — Operations Intelligence Dashboard
## Built with Microsoft Power BI | Multi-Table Logistics Dataset

![Dashboard - Preview](https://github.com/simplysmarty/Translogix-freight-operations-dashboard/blob/main/data/Intelligence%20Dashboard.png)

---

## 🏢 Company Overview

**TransLogix Freight Company** is a large-scale trucking and freight logistics company operating a fleet of **120 trucks**, **180 trailers** and **150 drivers** across the United States. The company moves freight across **58 routes** connecting major commercial cities, serving **200 customers** across three load types — Dry Van and Refrigerated — through Direct, Broker and Contract booking channels.

Over a **three-year operating period (2022–2024)**, TransLogix processed **85,410 loads**, drove **122.2 million miles** and generated **$262.5 million in base revenue**. Despite these impressive headline figures, the company faced mounting pressure from rising fuel costs, declining on-time delivery performance, inconsistent fleet utilisation and a series of costly safety incidents — all without a structured analytical framework to diagnose and address the root causes.

---

## 📌 Business Problem Statement

> *"TransLogix Freight Company generates $262.5 million in base revenue annually but operates without structured visibility into the drivers of its profitability, efficiency and safety performance. Fuel costs at $95.6 million represent 36.4% of total base revenue, the single largest cost in the business yet the company has no insight into which routes, drivers or states are consuming fuel most inefficiently. The on-time delivery rate stands at only 55.7%, meaning nearly 1 in every 2 deliveries arrives late, threatening customer retention and contract renewals. Meanwhile the company has no formal framework to identify which drivers, trucks and routes are generating the most value versus creating the most risk.*
>
> *Management needs a comprehensive operational intelligence dashboard that connects revenue performance, fuel efficiency, driver productivity, fleet utilisation, maintenance cost and safety management into one integrated analytical view so that data-driven decisions can be made immediately about where to invest, where to cut costs and where to intervene before problems compound."*

— **Mr. Ezekiel Adeleke, Operations Director, TransLogix Freight Company**

---

## 📊 Dashboard Preview

### Page 1 — Executive Overview
![Executive Overview](data/Page%201%20Executive%20Overview.png)

### Page 2 — Operational Efficiency
![Operational Efficiency](data/Page%202%20Operational%20Efficiency.png)

### Page 3 — Driver & Fleet Performance
![Driver and Fleet](data/Page%203%20Driver%20Fleet.png)

### Page 4 — Safety Dashboard
![Safety Dashboard](data/Page%204%20Safety.png)

---

## 🗂️ Dataset Overview

This capstone project uses **14 interconnected tables** containing over **500,000 rows** of transactional data across a three-year operating period.

| Table | Description | Rows |
|---|---|---|
| `loads` | Every shipment booked — revenue, weight, customer, route | 85,410 |
| `trips` | Every journey made — distance, fuel, duration, driver, truck | 85,410 |
| `drivers` | Driver profiles — experience, status, home terminal | 150 |
| `driver_monthly_metrics` | Monthly performance per driver — MPG, OTD, revenue | 4,464 |
| `trucks` | Fleet details — make, model year, status, terminal | 120 |
| `truck_utilization_metrics` | Monthly truck performance — utilisation, downtime, revenue | 3,312 |
| `trailers` | Trailer fleet details — type, status | 180 |
| `routes` | All routes — origin, destination, distance, rate per mile | 58 |
| `customers` | Customer profiles — type, revenue potential, account status | 200 |
| `delivery_events` | Every pickup and delivery — on-time flag, detention minutes | 170,820 |
| `fuel_purchases` | Every fuel stop — gallons, price, state, truck | 196,442 |
| `maintenance_records` | Every maintenance event — type, cost, downtime hours | 2,920 |
| `safety_incidents` | Every accident and incident — cost, fault, injury flag | 170 |
| `facilities` | Warehouses and depots — location, type, capacity | 50 |

**Total records across all tables: ~630,000+**

---

## 🔗 Data Model — Table Relationships

```
Date Table[Date]
      │
      ├──► loads[load_date]          ◄── customers[customer_id]
      │         │                    ◄── routes[route_id]
      │         │
      │         └──► trips[load_id]  ◄── drivers[driver_id]
      │                   │          ◄── trucks[truck_id]
      │                   │          ◄── trailers[trailer_id]
      │                   │
      │                   ├──► delivery_events[trip_id]
      │                   ├──► fuel_purchases[trip_id]
      │                   └──► safety_incidents[trip_id]
      │
      ├──► fuel_purchases[purchase_date]
      ├──► maintenance_records[maintenance_date]
      ├──► safety_incidents[incident_date]
      ├──► driver_monthly_metrics[month]
      └──► truck_utilization_metrics[month]
```

**Relationship types:** All relationships are Many-to-One from the fact tables to their dimension tables, with single-direction cross-filtering. The Date Table is marked as the official date table and connects to all seven date columns across the model.

---

## 🔍 Insight Questions

### Page 1 — Executive Overview
1. Which customer type generates the most revenue — Contract, Spot or Dedicated?
2. Who are the top 10 customers by base revenue and are they active or at risk of losing their account?
3. Which booking type drives the most total revenue and which is the most efficient per individual load?
4. How has base revenue trended month by month from 2022 to 2024?
5. How has revenue trended separately for Dry Van and Refrigerated load types over the same period?

### Page 2 — Operational Efficiency
6. Which routes generate the highest revenue and which are the most efficient per mile driven?
7. Which states carry the highest total fuel costs and where should the company prioritise refuelling optimisation?
8. What proportion of total base revenue is consumed by fuel costs — and is that proportion sustainable?
9. How has total fuel spend trended month by month from 2022 to 2024?
10. Which truck make delivers the highest average fuel efficiency (MPG) across the fleet?

### Page 3 — Driver & Fleet Performance
11. Which drivers generate the most revenue and have the best on-time delivery rates?
12. Which drivers have the highest fuel efficiency (MPG) and what can the rest of the fleet learn from them?
13. Is there a measurable relationship between a driver's years of experience and their on-time delivery performance?
14. Which trucks have the highest utilisation rates and which are chronically underperforming?
15. Which maintenance type costs the most and causes the most operational downtime?
16. Is there a relationship between truck age and total maintenance cost?

### Page 4 — Safety Dashboard
17. What percentage of all incidents were at-fault and what percentage were preventable?
18. Which drivers have been involved in the most safety incidents — and how many of their incidents were preventable?
19. Which incident type generates the highest total cost in vehicle damage, cargo damage and insurance claims?
20. Is there a geographic pattern to safety incidents — do specific cities or states have disproportionately high incident rates?

---

## 🧹 Data Cleaning Process

All cleaning was performed in **Power Query** before loading data into the Power BI data model.

### Step 1 — Date Column Conversion
All date columns across all tables were stored as text strings in the raw CSV files. Each was converted to the proper **Date** data type:
- `loads[load_date]`
- `trips[dispatch_date]`
- `fuel_purchases[purchase_date]`
- `maintenance_records[maintenance_date]`
- `safety_incidents[incident_date]`
- `driver_monthly_metrics[month]`
- `truck_utilization_metrics[month]`

### Step 2 — Null Value Handling
- **trips[driver_id]** — 1,714 null values replaced with `"Unassigned"` to prevent broken relationships
- **trips[truck_id]** — 1,672 null values replaced with `"Unassigned"`
- **fuel_purchases[truck_id]** — 3,880 null values replaced with `"Unknown"`
- **fuel_purchases[driver_id]** — 3,988 null values replaced with `"Unknown"`

### Step 3 — Driver Status Column
The `drivers[termination_date]` column contained nulls for all active drivers. A conditional column was added:
- If `termination_date` is null → `"Active"`
- Otherwise → `"Terminated"`

### Step 4 — Truck Age Calculated Column
Added a calculated column to the `trucks` table for use in maintenance cost analysis:
```
Truck Age = 2024 - trucks[model_year]
```

### Step 5 — Route Label Column
A concatenated route label was created for display in charts:
```
Route Label = routes[origin_city] & " → " & routes[destination_city]
```

### Step 6 — Driver Full Name Column
Combined first and last name for driver identification in charts:
```
Driver Full Name = drivers[first_name] & " " & drivers[last_name]
```

### Step 7 — Date Table Creation
A dedicated Date Table was created to enable all time intelligence functions:
```dax
Date Table = CALENDAR(DATE(2022, 1, 1), DATE(2024, 12, 31))
```
With calculated columns: `Year`, `Month Number`, `Month Name`, `Quarter`, `Year Month`, `Year Quarter`. The table was marked as the official date table and connected to all seven date columns across the model.

### Step 8 — Delivery Events Date Extraction
The `delivery_events[scheduled_datetime]` and `actual_datetime` columns contained full datetime values. Date-only columns were extracted:
- `scheduled_date = DATE(delivery_events[scheduled_datetime])`
- `actual_date = DATE(delivery_events[actual_datetime])`

---

## 📐 DAX Measures

### Core Revenue Measures
```dax
Base Revenue = SUM('loads'[revenue])

Total Revenue = 
    SUM('loads'[revenue])
    + SUM('loads'[fuel_surcharge])
    + SUM('loads'[accessorial_charges])

Total Loads = COUNTROWS('loads')

Total Miles = SUM('trips'[actual_distance_miles])

Revenue Per Load = DIVIDE([Base Revenue], [Total Loads])

Revenue Per Mile = DIVIDE([Base Revenue], [Total Miles])
```

### Fleet & Driver Measures
```dax
Active Drivers = 
    CALCULATE(COUNTROWS('drivers'), 
    'drivers'[employment_status] = "Active")

Active Trucks = 
    CALCULATE(COUNTROWS('trucks'), 
    'trucks'[status] = "Active")

Avg MPG = AVERAGE('trips'[average_mpg])

Avg Truck Utilization = 
    AVERAGE('truck_utilization_metrics'[utilization_rate])

Total Downtime Hours = 
    SUM('truck_utilization_metrics'[downtime_hours])

Driver Revenue = SUM('driver_monthly_metrics'[total_revenue])

Driver Avg OTD Rate = 
    AVERAGE('driver_monthly_metrics'[on_time_delivery_rate])
```

### Fuel Measures
```dax
Total Fuel Cost = SUM('fuel_purchases'[total_cost])

Avg Price Per Gallon = AVERAGE('fuel_purchases'[price_per_gallon])

Total Gallons Used = SUM('trips'[fuel_gallons_used])

Fuel As Pct Revenue = 
    DIVIDE([Total Fuel Cost], [Base Revenue])

Net Revenue After Fuel = [Base Revenue] - [Total Fuel Cost]

Fuel Cost Per Mile = DIVIDE([Total Fuel Cost], [Total Miles])
```

### Delivery Performance Measures
```dax
On Time Delivery Rate = 
    DIVIDE(
        CALCULATE(COUNTROWS('delivery_events'),
        'delivery_events'[on_time_flag] = TRUE()),
        COUNTROWS('delivery_events'))

Late Deliveries = 
    CALCULATE(COUNTROWS('delivery_events'),
    'delivery_events'[on_time_flag] = FALSE())

Total Detention Minutes = SUM('delivery_events'[detention_minutes])

Avg Detention Minutes = AVERAGE('delivery_events'[detention_minutes])
```

### Safety Measures
```dax
Total Incidents = COUNTROWS('safety_incidents')

Preventable Incidents = 
    CALCULATE(COUNTROWS('safety_incidents'),
    'safety_incidents'[preventable_flag] = TRUE())

Preventable Rate = 
    DIVIDE([Preventable Incidents], [Total Incidents])

At Fault Incidents = 
    CALCULATE(COUNTROWS('safety_incidents'),
    'safety_incidents'[at_fault_flag] = TRUE())

At Fault Rate = DIVIDE([At Fault Incidents], [Total Incidents])

Incidents With Injury = 
    CALCULATE(COUNTROWS('safety_incidents'),
    'safety_incidents'[injury_flag] = TRUE())

Total Claims Cost = SUM('safety_incidents'[claim_amount])

Avg Claim Per Incident = 
    DIVIDE(SUM('safety_incidents'[claim_amount]),
    COUNTROWS('safety_incidents'))
```

### Maintenance Measures
```dax
Total Maintenance Cost = SUM('maintenance_records'[total_cost])

Total Maintenance Downtime = SUM('maintenance_records'[downtime_hours])

Avg Maintenance Cost Per Event = 
    DIVIDE(SUM('maintenance_records'[total_cost]),
    COUNTROWS('maintenance_records'))

Maintenance Cost Per Mile = 
    DIVIDE(SUM('maintenance_records'[total_cost]), [Total Miles])
```

### Year-Over-Year Measures
```dax
Revenue PY = 
    CALCULATE([Base Revenue],
    SAMEPERIODLASTYEAR('Date Table'[Date]))

Revenue YoY % = 
    VAR Current = [Base Revenue]
    VAR Previous = [Revenue PY]
    RETURN
        IF(ISBLANK(Previous), BLANK(),
        DIVIDE(Current - Previous, Previous))

Revenue YoY Text = 
    VAR YoYPct = [Revenue YoY %]
    VAR Arrow = IF(YoYPct >= 0, "▲ ", "▼ ")
    RETURN
        IF(ISBLANK(YoYPct), "No prior year data",
        Arrow & FORMAT(ABS(YoYPct), "0.0%") & " YoY")

Revenue YoY Color = 
    IF([Revenue YoY %] >= 0, "#2ECC71", "#E74C3C")

-- Applied the same pattern for:
-- Fuel Cost YoY (inverted — increase = red)
-- On Time Rate YoY
-- Total Loads YoY
-- Total Miles YoY
-- Maintenance Cost YoY (inverted — increase = red)
-- Total Incidents YoY (inverted — increase = red)
-- Claims Cost YoY (inverted — increase = red)
```

### Dynamic Insight Text Measures
```dax
-- Example: Route Performance Insight
Scatter Insight Text = 
    VAR StarCount =
        COUNTROWS(FILTER(ALL('routes'),
            'routes'[Route Performance Category] = "Star Route"))
    VAR UnderCount =
        COUNTROWS(FILTER(ALL('routes'),
            'routes'[Route Performance Category] = "Underperformer"))
    RETURN
        FORMAT(StarCount, "0")
        & " Star Routes are driving the most value — "
        & FORMAT(UnderCount, "0")
        & " routes are underperforming and need review"

-- Example: Fuel Efficiency by Make
MPG By Make Insight = 
    VAR BestMake =
        MAXX(TOPN(1, SUMMARIZE('trucks', 'trucks'[make],
            "AvgMPG", AVERAGE('truck_utilization_metrics'[average_mpg])),
            [AvgMPG], DESC), 'trucks'[make])
    VAR BestMPG =
        MAXX(TOPN(1, SUMMARIZE('trucks', 'trucks'[make],
            "AvgMPG", AVERAGE('truck_utilization_metrics'[average_mpg])),
            [AvgMPG], DESC), [AvgMPG])
    RETURN
        BestMake & " leads fleet efficiency at "
        & FORMAT(BestMPG, "0.00") & " MPG"
```

### Risk Scoring Measures
```dax
-- Route Risk Score (0 to 1 composite)
Route Risk Score = 
    VAR IncidentWeight = 0.35
    VAR ClaimsWeight = 0.30
    VAR LateRateWeight = 0.25
    VAR FuelRatioWeight = 0.10
    RETURN
        ([Route Incident Score] * IncidentWeight)
        + ([Route Claims Score] * ClaimsWeight)
        + ([Route Late Rate Score] * LateRateWeight)
        + ([Route Fuel Ratio Score] * FuelRatioWeight)

-- Driver Risk Score (0 to 1 composite)
Driver Risk Score = 
    VAR IncidentWeight = 0.35
    VAR PreventableWeight = 0.30
    VAR OTDWeight = 0.20
    VAR IdleWeight = 0.15
    RETURN
        ([Driver Incident Score] * IncidentWeight)
        + ([Driver Preventable Score] * PreventableWeight)
        + ([Driver OTD Risk Score] * OTDWeight)
        + ([Driver Idle Score] * IdleWeight)

-- Risk Labels
Route Risk Label = 
    IF([Route Risk Score] >= 0.70, "🔴 High Risk",
    IF([Route Risk Score] >= 0.40, "🟡 Medium Risk",
    "🟢 Low Risk"))

Driver Risk Label = 
    IF([Driver Risk Score] >= 0.70, "🔴 High Risk",
    IF([Driver Risk Score] >= 0.40, "🟡 Medium Risk",
    "🟢 Low Risk"))
```

---

## 💡 Key Findings

### Finding 1 — Fuel is Consuming 36.4% of Every Revenue Dollar
Total fuel cost across the three-year period was **$95.6 million** against **$262.5 million** in base revenue. For every $1.00 the company earns, **$0.36 goes directly to fuel**. This is the single most material cost in the entire business — larger than maintenance, safety claims and driver compensation combined. Any meaningful improvement in fleet fuel efficiency has a direct and immediate impact on net profitability.

### Finding 2 — Nearly Half of All Deliveries Are Late
The on-time delivery rate across **170,820 delivery events** was just **55.7%**, meaning **44.3% of all deliveries arrived late**. Average detention time was **91.5 minutes per event** — amounting to **15.6 million minutes** of total detention across the period. In a competitive freight market where contract renewals depend on service reliability, a late rate above 40% is a serious and urgent customer retention risk.

### Finding 3 — Long-Haul Routes Dominate Revenue
The top two revenue-generating routes — **Philadelphia to Seattle ($10.07M)** and **Charlotte to Portland ($9.98M)** are both cross-country hauls. Long-distance routes earn significantly higher revenue per load ($6,873 average) compared to shorter regional routes. However, these same routes also carry higher fuel costs and more safety incident exposure, making the **Revenue vs Efficiency Per Mile scatter analysis** essential for correctly identifying true profitability.

### Finding 4 — Revenue is Nearly Equally Split Between Load Types
Base revenue was split almost evenly between **Refrigerated ($131.97M, 50.3%)** and **Dry Van ($130.56M, 49.7%)** loads. This balance appears stable across all three years. The key analytical question is whether this balance reflects a deliberate strategic decision or a missed opportunity to shift capacity toward whichever load type is more profitable per mile after accounting for fuel and maintenance differences.

### Finding 5 — Month-to-Month Seasonal Patterns Are Significant
Revenue and load volume showed consistent seasonal patterns across 2022–2024. **Q4 (October–December)** was consistently the strongest period while **Q1 (January–March)** was the weakest. This predictable seasonality represents an operational planning opportunity — fleet maintenance, driver scheduling and inventory positioning can all be optimised around the known demand cycle rather than reacting to it.

### Finding 6 — Fleet Utilisation Averages 83% But Downtime is Material
Average truck utilisation across the fleet was **83.0%** — a healthy headline figure. However, **54,812 hours of total downtime** across all trucks represents a significant pool of lost revenue-generating time. At an average revenue rate of approximately $2.15 per mile and an estimated average speed, even recovering 10% of that downtime through better maintenance scheduling would generate millions in additional revenue annually.

### Finding 7 — Equipment Damage and DOT Violations Are the Most Costly Incidents
Of the **$2.65 million** in total safety claims across 170 incidents, **equipment damage ($741K)** and **DOT violations ($581K)** were the two largest cost categories. DOT violations are entirely preventable through compliance management — they represent a direct and immediate cost reduction opportunity requiring no capital investment, only process improvement.

### Finding 8 — 37.6% of All Safety Incidents Were Preventable
Of 170 total incidents, **64 (37.6%)** were classified as preventable and **54 (31.8%)** were at-fault. A total of **33 incidents involved injuries**. The preventable incidents represent a significant controllable cost — the claims associated with preventable incidents alone are estimated at approximately **$996K**, meaning better safety training and driver management could recover nearly $1 million in annual claims cost.

### Finding 9 — The Highest Risk Drivers and Routes Are Identifiable
Using a composite risk scoring model combining incident count, preventable rate, on-time delivery rate and idle hours, the dashboard identifies specific **high-risk drivers (score ≥ 0.70)** and **high-risk routes (score ≥ 0.70)** that require immediate management intervention. These are not just drivers with the most incidents — they are drivers whose pattern of incidents, idle time and OTD performance creates a compound risk profile that a single metric would miss.

### Finding 10 — Truck Make Significantly Affects Fuel Efficiency
Average MPG varied meaningfully across truck makes in the fleet. The most fuel-efficient make outperformed the least efficient by a margin sufficient to justify factoring manufacturer fuel efficiency data into future fleet procurement decisions. Given that fuel represents 36.4% of revenue, a 0.5 MPG improvement across the full fleet of 120 trucks would generate millions in annual fuel savings.

---

## ✅ Recommendations

**Recommendation 1 — Launch an Immediate Fuel Efficiency Programme**
With fuel consuming 36.4% of base revenue, this is the highest-return intervention available. Three specific actions: (1) mandate driver idle reduction targets for the 15 highest-idle drivers identified on the Driver Performance scatter chart, (2) prioritise route assignments toward the highest-MPG truck makes for long-haul routes, and (3) establish monthly MPG benchmarks per driver with performance tied to incentive pay. A 0.5 MPG fleet-wide improvement would recover approximately $3–5M annually.

**Recommendation 2 — Address the On-Time Delivery Crisis Before the Next Contract Renewal Cycle**
A 44.3% late delivery rate is commercially unsustainable. The retention team should immediately identify which of the top 10 customers by revenue have experienced the worst OTD rates — these customers are at the highest contract renewal risk. Simultaneously, the operations team should investigate the root causes at the facilities with the highest detention times, as excessive detention is frequently a scheduling and dock management problem with a straightforward process fix.

**Recommendation 3 — Implement DOT Compliance Training to Eliminate the Second Largest Incident Cost**
DOT violations at $581K in claims require no capital investment to fix. A structured quarterly compliance review programme including pre-trip inspection checklists, HOS (Hours of Service) monitoring and load securement training would address the root causes. This is the fastest return-on-investment safety intervention available to the company.

**Recommendation 4 — Reassign or Retrain the Highest-Risk Drivers**
The composite Driver Risk Score identifies specific individuals whose combination of incidents, preventable rate, OTD failure and excessive idle time represents a disproportionate cost and liability burden. These drivers should receive individual performance improvement plans with defined 90-day targets. Drivers who do not improve should be considered for route reassignment to lower-risk, shorter-distance lanes.

**Recommendation 5 — Shift Route Investment Toward Star Routes**
The Route Performance scatter chart identifies routes that are both high-revenue and high-efficiency per mile. These Star Routes should receive priority truck allocation, the most experienced and highest-performing drivers, and the first access to maintenance slots to minimise downtime. Underperforming routes — low revenue and low efficiency — should be reviewed for commercial renegotiation or elimination from the route portfolio.

**Recommendation 6 — Begin a Structured Fleet Renewal Plan for the Oldest Trucks**
The truck age versus maintenance cost scatter analysis confirms that older trucks drive significantly higher maintenance spend. Trucks beyond a defined age threshold should be flagged for planned replacement in the capital budget cycle — the ongoing maintenance cost of keeping aging vehicles in service is demonstrably higher than the depreciation cost of newer equipment.

**Recommendation 7 — Leverage Seasonal Patterns for Operational Planning**
Q4 demand spikes and Q1 demand troughs are consistent and predictable. The company should schedule the majority of preventive maintenance activity in Q1 when demand is lowest — reducing Q4 downtime when every truck is needed. Driver leave scheduling, training programmes and facility upgrades should all be concentrated in the January–March window.

---

## 🌍 Implications

**For the Operations Team:**
The on-time delivery finding has immediate contractual implications. Customers with dedicated service agreements who have experienced persistent late deliveries may have grounds for service level penalties or early contract termination. Proactive communication and service recovery plans should be initiated before the next formal review.

**For the Finance Team:**
Fuel at 36.4% of revenue means the business is highly exposed to fuel price volatility. The fuel cost trend analysis shows that cost increases have come from both volume growth and price per gallon increases. A fuel hedging strategy should be evaluated to reduce the company's exposure to spot market price swings.

**For the Safety & Compliance Team:**
The preventability analysis reveals that over one-third of all incidents could have been avoided. Beyond the direct claims cost, preventable incidents create regulatory exposure, insurance premium increases and reputational risk with shippers. A safety culture investment including driver recognition programmes for zero-incident performance would address the behavioural root causes that compliance enforcement alone cannot reach.

**For Human Resources:**
The driver performance data reveals a consistent relationship between driver tenure and reliability metrics. This has direct implications for driver recruitment, onboarding programmes and retention strategy. Investing in keeping experienced, high-performing drivers reduces both operational risk and the cost of constant driver replacement and retraining.

**For Commercial Strategy:**
The customer revenue analysis shows that a small number of customers generate a disproportionate share of total revenue. The company's service improvement investments on-time delivery, detention reduction, account management should be explicitly prioritised toward these highest-value accounts to protect the revenue base that sustains the entire operation.

---

## 🏁 Conclusions

This analysis demonstrates that TransLogix Freight Company is a commercially strong business with significant operational inefficiencies that are preventable, measurable and correctable.

The headline revenue of $262.5 million across 85,410 loads reflects genuine market capability. However, the combination of a 36.4% fuel cost burden, a 44.3% late delivery rate and $2.65 million in safety claims means the company is operating well below its profitability potential. None of these problems require new markets, new customers or new capital investment to solve but they require data-driven operational discipline applied consistently to the areas the analysis has already identified.

The four-page dashboard built for this project goes beyond standard reporting. It integrates 14 data sources into a single analytical framework that enables management to ask and answer operational questions that were previously invisible. The composite risk scoring models for routes, the fuel efficiency quadrant analysis, the scatter chart correlations between experience and performance, and the seasonal pattern identification all represent analytical approaches that move the business from reactive management to proactive intelligence.

The most important conclusion from this analysis is not any individual finding — it is that **TransLogix already has the data it needs to significantly improve its profitability, safety record and service quality**. The bottleneck has never been the data. It has been the absence of a structured analytical framework to turn that data into decisions.

This dashboard provides that framework.

---

## 🛠️ Tools & Technologies Used

| Tool | Purpose |
|---|---|
| Microsoft Power BI Desktop | Dashboard design, data modelling and visualisation |
| Power Query (M Language) | Data cleaning, transformation and column creation |
| DAX (Data Analysis Expressions) | All calculated measures, columns and KPIs |
| 14 CSV source files | Raw operational data across all business functions |

---

## 🔧 Data Cleaning Summary

| Issue | Table | Action Taken |
|---|---|---|
| Date columns stored as text | All 7 date columns | Converted to Date type in Power Query |
| 1,714 null driver IDs | trips | Replaced with "Unassigned" |
| 1,672 null truck IDs | trips | Replaced with "Unassigned" |
| 3,880 null truck IDs | fuel_purchases | Replaced with "Unknown" |
| 3,988 null driver IDs | fuel_purchases | Replaced with "Unknown" |
| No driver status column | drivers | Added conditional column from termination_date |
| No truck age column | trucks | Added calculated column: 2024 - model_year |
| Datetime in delivery events | delivery_events | Extracted date-only columns |
| No route display label | routes | Concatenated origin → destination |

---

## 📂 Dataset Source

This dataset was provided as part of the **TS Academy Data Analytics Capstone Project**, curated by **Mr. Ezekiel Adeleke**. The data represents a realistic simulation of a US-based trucking and freight logistics operation across a three-year period.

---

## 👤 About the Analyst

**Fasanya Segun - Data Analyst, Lagos Nigeria**

Open to remote roles — Junior Data Analyst · BI Analyst · People Analytics · Reporting Analyst





[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=flat&logo=linkedin)](www.linkedin.com/in/segun-fasanya-879a943b1)
[![Twitter](https://img.shields.io/badge/Twitter-Follow-1DA1F2?style=flat&logo=twitter)](https://x.com/simplyselected)
[![GitHub](https://img.shields.io/badge/GitHub-Portfolio-181717?style=flat&logo=github)](https://github.com/simplysmarty)

---

*This project was completed as the TS Academy Data Analytics Capstone submission. Every step of the analytical process from data modelling to DAX to dashboard design was documented publicly as part of an ongoing commitment to learning in public.*
