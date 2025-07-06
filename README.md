# SQL_Bicycle Manufacturing & Business Optimization
Use SQL for exploring Bicycle Manufacturer Dataset
![image](https://github.com/user-attachments/assets/589db91a-6188-4d1f-899b-66255b1e3fca)


- Author: Nguyen Thuy Hang
- Date: 2025-06-22
- Tools Used: SQL  

# 📑 Table of Contents  
1. [📌 Background & Overview](#-background--overview)  
2. [📂 Dataset Description & Data Structure](#-dataset-description--data-structure)  
3. [🔎 Exploring dataset & Insights](#-exploring-dataset-&-insights)

---

## 📌 Background & Overview  
### Background
Adventure Works is a well-known company that manufactures and sells bicycles, parts and accessories.

### Objective:
### 📖 What is this project about? What Business Question will it solve?

This project is to analyze product, customer, region, and inventory performance across time to uncover growth opportunities, improve operational efficiency, and support data-driven decisions in sales, marketing, and supply chain planning.

The main business question this project aims to answer is: "How can Adventure Works optimize product sales, customer retention, regional performance, and inventory management to drive growth and improve operational efficiency?"


### 👤 Who is this project for?  
This project is designed for Adventure Works’ business leaders and decision-makers, including sales managers, marketing teams, supply chain planners, and operations executives who need actionable insights to boost sales, improve customer loyalty, optimize inventory, and expand market presence effectively.

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source: The dataset originates from the Google Analytics public dataset and represents information from AdventureWorks company.

### 📊 Data Structure & Relationships  

Dataset: https://dataedo.com/samples/html/AdventureWorks/ 

This project use 6 tables from this dataset: 
- SalesOrderDetail
- SalesOrderHeader
- SalesSpeacialOffer
- production.product
- production.productsubcategory
- production.workorder

## ⚒️ Main Process

1️⃣ Understand dataset 

2️⃣ Explore specific dataset through some requirements

---

## 🔎 Exploring dataset & insights  

**QUERY 1 - Calculate Quantity of items, Sales value & Order quantity by each Subcategory in L12M**

*Purpose*: Evaluate sales performance by Subcategory (volume, revenue, orders) over the last 12 months to identify best-selling product groups and guide inventory, marketing, and sales strategy

```sql
SELECT    
    Format_date('%b %Y', detail.ModifiedDate) as period
    ,subcate.Name
    ,sum(OrderQty) as count_items
    ,count(distinct detail.SalesOrderID) as count_orders
    ,ROUND(sum(LineTotal),2) as sales
FROM `adventureworks2019.Sales.SalesOrderDetail` detail
LEFT JOIN `adventureworks2019.Production.Product` product
    ON detail.ProductID = product.ProductID
LEFT JOIN `adventureworks2019.Production.ProductSubcategory` subcate
    ON cast(product.ProductSubcategoryID as int) = subcate.ProductSubcategoryID
WHERE DATE(detail.ModifiedDate) BETWEEN DATE('2013-06-30') AND DATE('2014-06-30')
GROUP BY 1,2
ORDER BY 2,1
```
*Result*

- Top 5 products with highest Sales

| period | Name | count_items | count_orders | sales |
| --- | --- | --- | --- | --- |
| 2014-03 | Road Bikes | 2371 | 520 | 2,148,706.22 |
| 2014-03 | Touring Bikes | 1952 | 305 | 1,973,165.63 |
| 2014-03 | Mountain Bikes | 1868 | 418 | 1,922,495.8 |
| 2014-05 | Touring Bikes | 1606 | 328 | 1,665,519.43 |
| 2014-05 | Mountain Bikes | 1336 | 483 | 1,578,293.99 |

- Top 5 products with lowest sales 

| period | Name | count_items | count_orders | sales |
| --- | --- | --- | --- | --- |
| 2013-09 | Locks | 1 | 1 | 15 |
| 2013-06 | Fenders | 2 | 2 | 43.96 |
| 2013-09 | Wheels | 1 | 1 | 83.3 |
| 2014-04 | Tights | 2 | 1 | 97.49 |
| 2013-07 | Bib-Shorts | 2 | 1 | 116.99 |


*Findings & Recommendations*
- Findings
  + Bikes dominate high sales: In Mar–May 2014, Road, Touring, and Mountain Bikes consistently led in both sales value and order volume, confirming strong seasonal demand in Q2.
  + Low performers are niche accessories: Items like Locks, Fenders, and Tights had minimal sales (≤ $100) mostly in mid-2013 to early 2014, showing weak or stagnant movement.
  + Sales spread is wide: The gap between top and bottom performers highlights clear category-level differences in demand and inventory value risk.
- Recommendations**
  + Focus Q2 campaigns on top-selling bike categories to maximize returns during peak season.
  + Review and rationalize underperforming SKUs like Locks and Fenders to reduce deadstock risk.
  + Use subcategory performance trends to guide inventory allocation and prioritize marketing spend by season.


**QUERY 2 - Calculate % YoY growth rate by SubCategory & release top 3 cat with highest growth rate. Can use metric: quantity_item. Round results to 2 decimal**

*Purpose*: Identify top 3 fastest-growing product categories to prioritize investment

```sql
with 
sale_info as (
  SELECT 
      FORMAT_TIMESTAMP("%Y", detail.ModifiedDate) as yr
      , subcate.Name
      , sum(detail.OrderQty) as qty_item

  FROM `adventureworks2019.Sales.SalesOrderDetail` detail
  LEFT JOIN `adventureworks2019.Production.Product` product on detail.ProductID = product.ProductID
  LEFT JOIN `adventureworks2019.Production.ProductSubcategory` subcate on cast(product.ProductSubcategoryID as int) = subcate.ProductSubcategoryID

  GROUP BY 1,2
  ORDER BY 2 asc , 1 desc
),

sale_diff as (
  select *
  , lead (qty_item) over (partition by Name order by yr desc) as prv_qty
  , round(qty_item / (lead (qty_item) over (partition by Name order by yr desc)) -1,2) as qty_diff
  from sale_info
  order by 5 desc 
),

rk_qty_diff as (
  select *
      ,dense_rank() over( order by qty_diff desc) as dk
  from sale_diff
)

select distinct Name
      , qty_item
      , prv_qty
      , qty_diff
      ,dk
from rk_qty_diff 
where dk <=3
order by dk ;
```
*Result*
| Name | qty_item | prv_qty | qty_diff | dk |
| --- | --- | --- | --- | --- |
| Mountain Frames | 3168 | 510 | 5.21 | 1 |
| Socks | 2724 | 523 | 4.21 | 2 |
| Road Frames | 5564 | 1137 | 3.89 | 3 |

*Findings & Recommendations*
- Findings
  + Mountain Frames shows the strongest growth, increasing over 5 times compared to the previous year, indicating a surge in demand
  + Socks and Road Frames also have very high growth, over 4 times and nearly 4 times respectively
  + -> These categories are the main drivers of Adventure Works’ sales growth.
- Recommendations
  + Prioritize production and inventory for these categories
  + Focus marketing and promotions on them
  + Explore product expansion or innovation to sustain growth.


**QUERY 3 - Ranking Top 3 TeritoryID with biggest Order quantity of every year. If there's TerritoryID with same quantity in a year, do not skip the rank number**

*Purpose*: Identify high-performing regions each year based on order volume to prioritize investment and market expansion in strong territories

```sql
with 
sale_info as (
  select 
      FORMAT_TIMESTAMP("%Y", detail.ModifiedDate) as year
      , sales_ordhead.TerritoryID
      , sum(OrderQty) as order_cnt 
  from `adventureworks2019.Sales.SalesOrderDetail` detail
  LEFT JOIN `adventureworks2019.Sales.SalesOrderHeader` sales_ordhead
    on detail.SalesOrderID = sales_ordhead.SalesOrderID
  group by 1,2
),

sale_rank as (
  select *
      , dense_rank() over (partition by sale_info.year order by order_cnt desc) as rank_sale 
  from sale_info 
)

select sale_rank.year
    , territoryID
    , order_cnt
    , rank_sale
from sale_rank 
where rank_sale in (1,2,3)   
;
```
*Result*
| year | territoryID | order_cnt | rank_sale |
| --- | --- | --- | --- |
| 2011 | 4 | 3238 | 1 |
| 2011 | 6 | 2705 | 2 |
| 2011 | 1 | 1964 | 3 |
| 2012 | 4 | 17553 | 1 |
| 2012 | 6 | 14412 | 2 |
| 2012 | 1 | 8537 | 3 |
| 2013 | 4 | 26682 | 1 |
| 2013 | 6 | 22553 | 2 |
| 2013 | 1 | 17452 | 3 |
| 2014 | 4 | 11632 | 1 |
| 2014 | 6 | 9711 | 2 |
| 2014 | 1 | 8823 | 3 |



*Findings & Recommendations*
- Findings
  + Territory 4 consistently leads in order volume across all years, showing strong and stable market performance.
  + Territory 6 holds the second position steadily but with fluctuating growth rates, indicating potential but less dominance than Territory 4.
  + Territory 1, while third, shows steady growth and could be an emerging market worth watching.
  + The gap between Territory 4 and others widened notably in 2012 and 2013, suggesting focused efforts or advantages in that region.
- Recommendations
  + Prioritize investment and tailored marketing strategies in Territory 4 to maintain and grow its market leadership.
  + Explore growth opportunities in Territory 6 by addressing its fluctuating performance and strengthening customer engagement.
  + Monitor Territory 1 closely as a rising market; consider pilot programs or targeted campaigns to accelerate growth.

**QUERY 4 - Calculate Total Discount Cost belongs to Seasonal Discount for each SubCategory**

*Purpose*: Measure seasonal discount costs by Subcategory to control promotion budget

```sql
select 
    FORMAT_TIMESTAMP("%Y", ModifiedDate)
    , Name
    , sum(disc_cost) as total_cost
from (
      select distinct detail.ModifiedDate
      , subcate.Name
      , specialoffer.DiscountPct, specialoffer.Type
      , detail.OrderQty * specialoffer.DiscountPct * UnitPrice as disc_cost 
      from `adventureworks2019.Sales.SalesOrderDetail` detail
      LEFT JOIN `adventureworks2019.Production.Product` product on detail.ProductID = product.ProductID
      LEFT JOIN `adventureworks2019.Production.ProductSubcategory` subcate on cast(product.ProductSubcategoryID as int) = subcate.ProductSubcategoryID
      LEFT JOIN `adventureworks2019.Sales.SpecialOffer` specialoffer on detail.SpecialOfferID = specialoffer.SpecialOfferID
      WHERE lower(specialoffer.Type) like '%seasonal discount%' 
)
group by 1,2;
```
*Result*
| f0_ | Name | total_cost |
| --- | --- | --- |
| 2012 | Helmets | 149.71669 |
| 2013 | Helmets | 543.21975 |

*Findings & Recommendations*
- Findings: Seasonal discount costs for Helmets surged over 3.5 times from 2012 ($150) to 2013 ($540) -> This indicates significant expansion of seasonal promotions or increased need to boost demand for this category.
- Recommendations
  + Optimize discount levels to balance sales uplift with profitability for Helmets.
  + Align discount timing and scale with inventory turnover to avoid excess stock buildup.
  + Establish clear budget limits for seasonal discounts by SubCategory to prevent overspending.

**QUERY 5 -- Retention rate of Customer in 2014 with status of Successfully Shipped (Cohort Analysis)**

*Purpose*: Assess customer retention through monthly repurchase behavior (cohort analysis) to understand loyalty trends, improve retention strategies, and reduce churn

```sql
with 
info as (
  select  
      extract(month from ModifiedDate) as month_no
      , extract(year from ModifiedDate) as year_no
      , CustomerID
      , count(Distinct SalesOrderID) as order_cnt
  from `adventureworks2019.Sales.SalesOrderHeader`
  where FORMAT_TIMESTAMP("%Y", ModifiedDate) = '2014'
  and Status = 5
  group by 1,2,3
  order by 3,1 
),

row_num as (
  select *
      , row_number() over (partition by CustomerID order by month_no) as row_numb
  from info 
), 

first_order as (
  select *
  from row_num
  where row_numb = 1
), 

month_gap as (
  select 
      info.CustomerID
      , first_order.month_no as month_join
      , info.month_no as month_order
      , info.order_cnt
      , concat('M - ',info.month_no - first_order.month_no) as month_diff
  from info 
  left join first_order 
  on info.CustomerID = first_order.CustomerID
  order by 1,3
)

select month_join
      , month_diff 
      , count(distinct CustomerID) as customer_cnt
from month_gap
group by 1,2
order by 1,2;
```
*Result*
| month_join | month_diff | customer_cnt |
| --- | --- | --- |
| 1 | M - 0 | 2076 |
| 1 | M - 1 | 78 |
| 1 | M - 2 | 89 |
| 1 | M - 3 | 252 |
| 1 | M - 4 | 96 |
| 1 | M - 5 | 61 |
| 1 | M - 6 | 18 |
| 2 | M - 0 | 1805 |
| 2 | M - 1 | 51 |
| 2 | M - 2 | 61 |
| 2 | M - 3 | 234 |
| 2 | M - 4 | 58 |
| 2 | M - 5 | 8 |
| 3 | M - 0 | 1918 |
| 3 | M - 1 | 43 |
| 3 | M - 2 | 58 |
| 3 | M - 3 | 44 |
| 3 | M - 4 | 11 |
| 4 | M - 0 | 1906 |
| 4 | M - 1 | 34 |
| 4 | M - 2 | 44 |
| 4 | M - 3 | 7 |
| 5 | M - 0 | 1947 |
| 5 | M - 1 | 40 |
| 5 | M - 2 | 7 |
| 6 | M - 0 | 909 |
| 6 | M - 1 | 10 |
| 7 | M - 0 | 148 |

*Findings & Recommendations*
- Findings
  + Customer retention sharply drops after the first month, with only a small fraction of customers repurchasing in subsequent months.
  + Some months (e.g., month 1 and 2 cohorts) show a temporary increase in repurchases at month 3, suggesting possible promotional or seasonal effects.
  + Overall, retention beyond month 3 is very low, indicating challenges in maintaining customer loyalty over time.
- Recommendations
  + Develop targeted engagement campaigns within the first 1–3 months post-purchase to boost early repurchase rates.
  + Analyze and replicate factors driving the month-3 repurchase spike to enhance longer-term retention.
  + Introduce loyalty programs or incentives aimed at sustaining repeat purchases beyond the initial 3 months.

**QUERY 6 - Trend of Stock level & MoM diff % by all product in 2011. If %gr rate is null then 0. Round to 1 decimal**

*Purpose*: Track monthly stock fluctuations to prevent overstock or stockouts

```sql
with 
raw_data as (
  select
      extract(month from work_order.ModifiedDate) as mth 
      , extract(year from work_order.ModifiedDate) as yr 
      , product.Name
      , sum(StockedQty) as stock_qty
  from `adventureworks2019.Production.WorkOrder` work_order
  left join `adventureworks2019.Production.Product` product on work_order.ProductID = product.ProductID
  where FORMAT_TIMESTAMP("%Y", work_order.ModifiedDate) = '2011'
  group by 1,2,3
  order by 1 desc 
)

select  Name
      , mth, yr 
      , stock_qty
      , stock_prv    
      , round(coalesce((stock_qty /stock_prv -1)*100 ,0) ,1) as diff    
from (
      select *
      , lead (stock_qty) over (partition by Name order by mth desc) as stock_prv
      from raw_data
      )
order by 1 asc, 2 desc;
```
*Result*: Top 5 products with highest stockqty_current below
| Name | month | year | stockqty_current | stockqty_previous | diff |
| --- | --- | --- | --- | --- | --- |
| BB Ball Bearing | 10 | 2011 | 19175 | 8845 | 116.8 |
| BB Ball Bearing | 11 | 2011 | 14544 | 19175 | -24.2 |
| BB Ball Bearing | 7 | 2011 | 12837 | 5259 | 144.1 |
| BB Ball Bearing | 8 | 2011 | 9666 | 12837 | -24.7 |
| Seat Stays | 10 | 2011 | 9340 | 4243 | 120.1 |

*Findings & Recommendations*
- Findings
  + Hidden Volatility in “BB Ball Bearing” Inventory
    ++ BB Ball Bearing appears 4 times in the top 5, suggesting it's a critical SKU with extremely unstable stock behavior.
    ++ Massive spikes in July (+144.1%) and October (+116.8%), followed by sharp drops in the next months (−24.7%, −24.2%).
    → This cyclical volatility hints at reactive inventory planning, possibly driven by inconsistent forecasting or sudden demand shifts.
  + Potential Overstock Risk on Seat Stays (Oct)
    ++ Seat Stays shows a sudden +120% surge in stock in October without further context.
    → If not aligned with demand, this could indicate overstocking, which ties up working capital and increases carrying costs.
  + Drop after Spike Pattern = Inefficient Replenishment
    ++ The repeated pattern of stock surge → sharp drop (seen in BB Ball Bearing) might indicate:
    Overstocking due to panic replenishment
    Then understocking as consumption catches up
    → This up-down cycle can create a bullwhip effect across the supply chain.
- Recommendations 
  + Control Replenishment for BB Ball Bearing
    ++ Add BB Ball Bearing to a monitored SKU list.
    ++ Set fluctuation threshold (±20%) to trigger review.
    ++ Require demand/sales justification before placing large POs.
  + Set Spike-Then-Drop Alerts
    ++ Implement rule: flag if stock ↑ >100% and ↓ >20% next month.
    ++ Trigger auto-alerts for Supply/Planning team.
    ++ Investigate cause and adjust reorder strategy if needed.
  + Require Demand Validation for Bulk Stock-Ins
    ++ Ensure all large POs are linked to: Forecast data, Sales campaigns, Confirmed customer orders
  + Add pre-check in PO approval workflow.


**QUERY 7 - -"Calculate Ratio of Stock / Sales in 2011 by product name, by month. Order results by month desc, ratio desc. Round Ratio to 1 decimal 
mom yoy**

*Purpose*: Analyze stock-to-sales ratio to identify inventory inefficiencies, detect slow-moving products, and optimize stock planning or clearance decisions

```sql
with 
sale_info as (
  select 
      extract(month from detail.ModifiedDate) as mth 
     , extract(year from detail.ModifiedDate) as yr 
     , detail.ProductId
     , product.Name
     , sum(detail.OrderQty) as sales
  from `adventureworks2019.Sales.SalesOrderDetail` detail
  left join `adventureworks2019.Production.Product` product
    on detail.ProductID = product.ProductID
  where FORMAT_TIMESTAMP("%Y", detail.ModifiedDate) = '2011'
  group by 1,2,3,4
), 

stock_info as (
  select
      extract(month from ModifiedDate) as mth 
      , extract(year from ModifiedDate) as yr 
      , ProductId
      , sum(StockedQty) as stock_cnt
  from `adventureworks2019.Production.WorkOrder`
  where FORMAT_TIMESTAMP("%Y", ModifiedDate) = '2011'
  group by 1,2,3
)

select
      sale_info.*
    , stock_info.stock_cnt as stock  --(*)
    , round(coalesce(stock_info.stock_cnt,0) / sales,2) as ratio
from sale_info 
full join stock_info 
  on sale_info.ProductId = stock_info.ProductId
and sale_info.mth = stock_info.mth 
and sale_info.yr = stock_info.yr
order by 1 desc, 7 desc;
```
*Result*

- Top 10 products with highest Ratio of Stock / Sales

| month | year | ProductId | Name | sales | stock | ratio |
| --- | --- | --- | --- | --- | --- | --- |
| 11 | 2011 | 761 | Road-650 Red, 62 | 1 | 56 | 56 |
| 10 | 2011 | 723 | LL Road Frame - Black, 60 | 1 | 46 | 46 |
| 9 | 2011 | 763 | Road-650 Red, 48 | 1 | 41 | 41 |
| 9 | 2011 | 761 | Road-650 Red, 62 | 1 | 34 | 34 |
| 11 | 2011 | 764 | Road-650 Red, 52 | 1 | 30 | 30 |
| 12 | 2011 | 745 | HL Mountain Frame - Black, 48 | 1 | 27 | 27 |
| 12 | 2011 | 743 | HL Mountain Frame - Black, 42 | 1 | 26 | 26 |
| 9 | 2011 | 771 | Mountain-100 Silver, 38 | 1 | 25 | 25 |
| 11 | 2011 | 772 | Mountain-100 Silver, 42 | 2 | 48 | 24 |
| 9 | 2011 | 773 | Mountain-100 Silver, 44 | 1 | 24 | 24 |


- Top 10 products with lowest Ratio of Stock / Sales

| month | year | ProductId | Name | sales | stock | ratio |
| --- | --- | --- | --- | --- | --- | --- |
| 8 | 2011 | 767 | Road-650 Black, 62 | 16 | 2 | 0.13 |
| 6 | 2011 | 751 | Road-150 Red, 48 | 28 | 4 | 0.14 |
| 8 | 2011 | 759 | Road-650 Red, 58 | 5 | 1 | 0.2 |
| 6 | 2011 | 752 | Road-150 Red, 52 | 12 | 4 | 0.33 |
| 8 | 2011 | 766 | Road-650 Black, 60 | 49 | 18 | 0.37 |
| 8 | 2011 | 765 | Road-650 Black, 58 | 70 | 27 | 0.39 |
| 8 | 2011 | 764 | Road-650 Red, 52 | 35 | 14 | 0.4 |
| 8 | 2011 | 768 | Road-650 Black, 44 | 42 | 17 | 0.4 |
| 10 | 2011 | 762 | Road-650 Red, 44 | 156 | 64 | 0.41 |
| 6 | 2011 | 750 | Road-150 Red, 44 | 23 | 10 | 0.43 |


*Findings & Recommendations*
- Findings 
  + Persistent Overstock of Slow-Moving SKUs
    ++ Products like Road-650 Red, 62 maintain extremely high stock-to-sales ratios (up to 56x) consistently from September to November 2011.
    -> This indicates that excess inventory is not being cleared effectively over time, suggesting outdated demand forecasts or insufficient markdown strategies.
  + Ongoing Low Stock Levels on High-Demand SKUs
    ++ Popular items such as Road-650 Black, 62 show low ratios (<0.2) in key months like June and August 2011, implying repeated risk of stockouts during peak demand.
    -> This points to unresolved supply chain or replenishment delays over multiple months.
  + Sustained Variant-Level Imbalance
    ++ Wide disparities in stock-to-sales ratios among similar SKUs across consecutive months reveal a lack of detailed, time-sensitive inventory planning.
    -> This likely causes recurring inefficiencies in how stock is allocated between variants.

- Recommendations
  + Implement Monthly SKU-Level Inventory Reviews: Regularly monitor stock-to-sales ratios for each variant to quickly identify persistent overstock or stockout issues and adjust replenishment plans accordingly.
  + Set Automated Alerts for Extreme Ratio Patterns: Create threshold-based alerts (e.g., ratio >20 for overstock, <0.5 for stockout risk) sustained over multiple months to prompt timely interventions such as promotions or expedited orders.
  + Align Procurement with Rolling Sales Data: Use a rolling 3-month sales average to dynamically guide purchasing decisions, ensuring supply closely matches evolving demand patterns and reducing inventory imbalances.


**QUERY 8 - Number of orders and value at Pending status in 2014**

*Purpose*: Monitor pending orders to uncover process bottlenecks, enhance fulfillment efficiency, and minimize revenue delays or customer dissatisfaction

```sql
SELECT
      EXTRACT(YEAR FROM ModifiedDate) AS year
      ,Status
      ,COUNT(DISTINCT PurchaseOrderID) AS order_cnt
      ,SUM(TotalDue) AS value
FROM `adventureworks2019.Purchasing.PurchaseOrderHeader`
WHERE Status = 1
      AND EXTRACT(YEAR FROM ModifiedDate) = 2014
GROUP BY year,Status
;
```
*Result*
| year | Status | order_cnt | value |
| --- | --- | --- | --- |
| 2014 | 1 | 224 | 3,873,579.0123 |

*Findings & Recommendations*
- Findings: In 2014, there were 224 pending orders totaling approximately $3.87 million in value -> This indicates a significant volume and monetary value stuck in the pending status, potentially delaying fulfillment and revenue recognition.
- Recommendations
  + Investigate causes of pending orders to identify and remove process bottlenecks
  + Streamline order processing workflows to reduce pending durations
  + Monitor pending order metrics regularly to prevent backlog buildup
  + Prioritize clearance of high-value pending orders to improve cash flow and customer satisfaction

### 💡Conclusion & Key Insights

Adventure Works is positioned for strong growth, driven by high-performing products and regions — but to fully capitalize, the company must **tighten inventory control, boost customer retention, and streamline operations**.

* **Core products (Bikes, Frames, Apparel)** and **top regions (Territory 4, 6, 1)** are consistently leading in sales and growth → *these are strategic anchors for future investment*.
* However, **stock volatility**, **low repeat purchase rates**, and **\$3.87M in pending orders** reveal inefficiencies that **erode profit and customer experience**.
  → To grow efficiently, **Adventure Works must align inventory, marketing, and fulfillment around what performs best — while fixing key operational bottlenecks.**

> **Sustain growth by focusing on best-sellers and strong markets — and unlock full potential by fixing retention, inventory, and fulfillment gaps.**










