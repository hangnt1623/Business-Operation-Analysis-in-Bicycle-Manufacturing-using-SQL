# Bicycle-Manufacturer
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

### Objective:
### 📖 What is this project about? What Business Question will it solve?

This project explores a Bicycle-Manufacturer dataset using SQL on Google BigQuery. It focuses on examining the data through basic queries to understand its structure and contents. The data originates from the Google Analytics public dataset.

The key business questions related to sales performance, stock trends, customer retention, and discount impact. It includes analysis of subcategory growth, territory rankings, stock-to-sales ratios, and pending orders. All insights are derived using SQL on the Bicycle-Manufacturer dataset.





### 👤 Who is this project for?  
This project is for *business analysts, operations managers, sales teams, and data analysts* who need actionable insights into product performance, customer retention, inventory levels, and sales trends to support data-driven decisions in the manufacturing business.

---

## 📂 Dataset Description & Data Structure  

### 📌 Data Source  
- Source: the dataset is obtained from Google Analytics public dataset 

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

**QUERY 1 - Calc Quantity of items, Sales value & Order quantity by each Subcategory in L12M**
```sql
SELECT
        FORMAT_DATE('%Y-%m', DATE_SUB(date(detail.ModifiedDate), INTERVAL 12 month)) AS month_year
        ,CAST(product.ProductSubcategoryID AS INT64) AS pro_sub_id
        ,subcate.Name
        ,SUM(detail.OrderQty) AS qty_item
        ,SUM(detail.LineTotal) AS total_sales
        ,COUNT(DISTINCT detail.SalesOrderID) AS order_cnt
FROM `adventureworks2019.Sales.SalesOrderDetail` AS detail
LEFT JOIN `adventureworks2019.Production.Product` AS product
        on product.ProductID = detail.ProductID
LEFT JOIN `adventureworks2019.Production.ProductSubcategory` AS subcate
        on CAST(product.ProductSubcategoryID AS INT64) = subcate.ProductSubcategoryID
WHERE DATE(detail.ModifiedDate) >= (
    SELECT DATE_SUB(MAX(DATE(detail.ModifiedDate)), INTERVAL 12 MONTH)
    FROM `adventureworks2019.Sales.SalesOrderDetail`)
GROUP BY month_year,detail.ProductID,product.ProductSubcategoryID,subcate.Name
ORDER BY month_year ASC
```


**QUERY 2 - Calc % YoY growth rate by SubCategory & release top 3 cat with highest grow rate. Can use metric: quantity_item. Round results to 2 decimal**
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


**QUERY 3 - Ranking Top 3 TeritoryID with biggest Order quantity of every year. If there's TerritoryID with same quantity in a year, do not skip the rank number**
```sql
with 
sale_info as (
  select 
      FORMAT_TIMESTAMP("%Y", detail.ModifiedDate) as yr
      , sales_ordhead.TerritoryID
      , sum(OrderQty) as order_cnt 
  from `adventureworks2019.Sales.SalesOrderDetail` detail
  LEFT JOIN `adventureworks2019.Sales.SalesOrderHeader` sales_ordhead
    on detail.SalesOrderID = sales_ordhead.SalesOrderID
  group by 1,2
),

sale_rank as (
  select *
      , dense_rank() over (partition by yr order by order_cnt desc) as rk 
  from sale_info 
)

select yr
    , TerritoryID
    , order_cnt
    , rk
from sale_rank 
where rk in (1,2,3)   --rk <=3
;
```


**QUERY 4 - Calc Total Discount Cost belongs to Seasonal Discount for each SubCategory**
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

**QUERY 5 -- Retention rate of Customer in 2014 with status of Successfully Shipped (Cohort Analysis)**
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



**QUERY 6 - Trend of Stock level & MoM diff % by all product in 2011. If %gr rate is null then 0. Round to 1 decimal**
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


**QUERY 7 - -"Calc Ratio of Stock / Sales in 2011 by product name, by month.Order results by month desc, ratio desc. Round Ratio to 1 decimal
mom yoy**
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


**QUERY 8 - No of order and value at Pending status in 2014**
```sql
SELECT
      EXTRACT(YEAR FROM ModifiedDate) AS yr
      ,Status
      ,COUNT(DISTINCT PurchaseOrderID) AS order_cnt
      ,SUM(TotalDue) AS value
FROM `adventureworks2019.Purchasing.PurchaseOrderHeader`
WHERE Status = 1
      AND EXTRACT(YEAR FROM ModifiedDate) = 2014
GROUP BY yr,Status
;
```


