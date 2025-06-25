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
ORDER BY month_year, subcate.Name
```
*Result*
| period | Name | count_items | count_orders | sales |
| --- | --- | --- | --- | --- |
| Apr 2014 | Bib-Shorts | 4 | 1 | 233.97 |
| Feb 2014 | Bib-Shorts | 4 | 2 | 233.97 |
| Jul 2013 | Bib-Shorts | 2 | 1 | 116.99 |
| Jun 2013 | Bib-Shorts | 2 | 1 | 116.99 |
| Apr 2014 | Bike Racks | 45 | 45 | 5,400 |
| Aug 2013 | Bike Racks | 222 | 63 | 17,387.18 |
| Dec 2013 | Bike Racks | 162 | 48 | 12,582.29 |
| Feb 2014 | Bike Racks | 27 | 27 | 3,2400 |
| Jan 2014 | Bike Racks | 161 | 53 | 12,840 |
| Jul 2013 | Bike Racks | 422 | 75 | 29,802.3 |
| Jun 2013 | Bike Racks | 363 | 57 | 24,684 |
| Jun 2014 | Bike Racks | 20 | 20 | 2,400 |
| Mar 2014 | Bike Racks | 492 | 100 | 35,588.71 |
| May 2014 | Bike Racks | 284 | 81 | 21,704.69 |
| Nov 2013 | Bike Racks | 142 | 50 | 11,472 |
| Oct 2013 | Bike Racks | 284 | 70 | 21,181.2 |
| Sep 2013 | Bike Racks | 312 | 71 | 22,828.51 |
| Apr 2014 | Bike Stands | 23 | 23 | 3,657 |
| Aug 2013 | Bike Stands | 20 | 20 | 3,180 |
| Dec 2013 | Bike Stands | 17 | 17 | 2,703 |
| Feb 2014 | Bike Stands | 17 | 17 | 2,703 |
| Jan 2014 | Bike Stands | 18 | 18 | 2,862 |
| Jul 2013 | Bike Stands | 19 | 19 | 3,021 |
| Jun 2013 | Bike Stands | 2 | 2 | 318 |
| Jun 2014 | Bike Stands | 11 | 11 | 1,749 |
| Mar 2014 | Bike Stands | 31 | 31 | 4,929 |
| May 2014 | Bike Stands | 13 | 13 | 2,067 |
| Nov 2013 | Bike Stands | 24 | 24 | 3,816 |
| Oct 2013 | Bike Stands | 24 | 24 | 3,816 |
| Sep 2013 | Bike Stands | 26 | 26 | 4,134 |
| Apr 2014 | Bottles and Cages | 734 | 435 | 5,239.66 |
| Aug 2013 | Bottles and Cages | 761 | 377 | 4,525.44 |
| Dec 2013 | Bottles and Cages | 793 | 431 | 5,158.66 |
| Feb 2014 | Bottles and Cages | 655 | 389 | 4,663.45 |
| Jan 2014 | Bottles and Cages | 869 | 472 | 5,729.75 |
| Jul 2013 | Bottles and Cages | 845 | 386 | 4,705.59 |
| Jun 2013 | Bottles and Cages | 356 | 64 | 1,071.11 |
| Jun 2014 | Bottles and Cages | 251 | 178 | 1,692.49 |
| Mar 2014 | Bottles and Cages | 1228 | 537 | 6,912.59 |
| May 2014 | Bottles and Cages | 1043 | 523 | 6,564.6 |
| Nov 2013 | Bottles and Cages | 894 | 482 | 5,883.57 |
| Oct 2013 | Bottles and Cages | 845 | 400 | 5,038.95 |
| Sep 2013 | Bottles and Cages | 803 | 380 | 4,676.56 |
| Aug 2013 | Bottom Brackets | 61 | 11 | 3,191.03 |
| Dec 2013 | Bottom Brackets | 40 | 19 | 2,915.76 |
| Jan 2014 | Bottom Brackets | 42 | 21 | 3,061.55 |
| Jul 2013 | Bottom Brackets | 155 | 36 | 8,787.57 |
| Jun 2013 | Bottom Brackets | 93 | 26 | 5,280.64 |
| Mar 2014 | Bottom Brackets | 118 | 29 | 5,928.49 |
| May 2014 | Bottom Brackets | 134 | 32 | 7,378.3 |
| Nov 2013 | Bottom Brackets | 24 | 9 | 1,749.46 |
| Oct 2013 | Bottom Brackets | 132 | 30 | 7,030.01 |
| Sep 2013 | Bottom Brackets | 60 | 19 | 3,118.14 |
| Aug 2013 | Brakes | 49 | 13 | 3,131.1 |
| Dec 2013 | Brakes | 32 | 16 | 2,044.8 |
| Jan 2014 | Brakes | 69 | 28 | 4,409.1 |
| Jul 2013 | Brakes | 205 | 42 | 13,099.5 |
| Jun 2013 | Brakes | 133 | 27 | 8,417.93 |
| Mar 2014 | Brakes | 107 | 39 | 6,837.3 |
| May 2014 | Brakes | 92 | 39 | 5,878.8 |
| Nov 2013 | Brakes | 26 | 8 | 1,661.4 |
| Oct 2013 | Brakes | 142 | 38 | 9,036.78 |
| Sep 2013 | Brakes | 100 | 29 | 6,390 |
| Apr 2014 | Caps | 193 | 193 | 1,735.07 |
| Aug 2013 | Caps | 354 | 188 | 2,450.51 |
| Dec 2013 | Caps | 301 | 210 | 2,281.66 |
| Feb 2014 | Caps | 180 | 180 | 1,618.2 |
| Jan 2014 | Caps | 365 | 243 | 2,727.68 |
| Jul 2013 | Caps | 526 | 214 | 3,331.74 |
| Jun 2013 | Caps | 415 | 59 | 2,161.84 |
| Jun 2014 | Caps | 93 | 93 | 836.07 |
| Mar 2014 | Caps | 663 | 277 | 4,219.75 |
| May 2014 | Caps | 456 | 281 | 3,294.21 |
| Nov 2013 | Caps | 319 | 221 | 2,429.28 |
| Oct 2013 | Caps | 406 | 208 | 2,738.84 |
| Sep 2013 | Caps | 440 | 203 | 2,879.48 |
| Aug 2013 | Chains | 28 | 12 | 340.03 |
| Dec 2013 | Chains | 39 | 16 | 473.62 |
| Jan 2014 | Chains | 64 | 28 | 777.22 |
| Jul 2013 | Chains | 156 | 32 | 1,886.79 |
| Jun 2013 | Chains | 91 | 23 | 1,098.07 |
| Mar 2014 | Chains | 93 | 32 | 1,129.39 |
| May 2014 | Chains | 84 | 29 | 1,020.1 |
| Nov 2013 | Chains | 24 | 9 | 291.46 |
| Oct 2013 | Chains | 93 | 31 | 1,122.36 |
| Sep 2013 | Chains | 62 | 24 | 752.93 |
| Apr 2014 | Cleaners | 88 | 88 | 699.6 |
| Aug 2013 | Cleaners | 241 | 99 | 1,340.63 |
| Dec 2013 | Cleaners | 202 | 119 | 1,262.46 |
| Feb 2014 | Cleaners | 52 | 52 | 413.4 |
| Jan 2014 | Cleaners | 182 | 100 | 1,110.24 |
| Jul 2013 | Cleaners | 344 | 118 | 1,830.32 |
| Jun 2013 | Cleaners | 339 | 58 | 1,546.57 |
| Jun 2014 | Cleaners | 42 | 42 | 333.9 |
| Mar 2014 | Cleaners | 454 | 138 | 2,349.56 |
| May 2014 | Cleaners | 336 | 139 | 1,889.06 |
| Nov 2013 | Cleaners | 202 | 111 | 1,230.66 |
| Oct 2013 | Cleaners | 294 | 108 | 1,612.76 |
| Sep 2013 | Cleaners | 296 | 108 | 1,611.83 |
| Aug 2013 | Cranksets | 60 | 13 | 10,448.64 |
| Dec 2013 | Cranksets | 55 | 17 | 10,610.67 |
| Jan 2014 | Cranksets | 85 | 27 | 17,900.49 |
| Jul 2013 | Cranksets | 191 | 39 | 36,294.95 |
| Jun 2013 | Cranksets | 149 | 23 | 27,766.23 |
| Mar 2014 | Cranksets | 131 | 30 | 23,294.81 |
| May 2014 | Cranksets | 123 | 34 | 20,937.76 |
| Nov 2013 | Cranksets | 32 | 10 | 6,949.61 |
| Oct 2013 | Cranksets | 118 | 33 | 19,722.79 |
| Sep 2013 | Cranksets | 75 | 20 | 13,955.85 |
| Aug 2013 | Derailleurs | 77 | 18 | 4,698.53 |
| Dec 2013 | Derailleurs | 38 | 17 | 2,085.97 |
| Jan 2014 | Derailleurs | 54 | 25 | 2,964.28 |
| Jul 2013 | Derailleurs | 215 | 42 | 12,989.02 |
| Jun 2013 | Derailleurs | 128 | 23 | 7,583.52 |
| Mar 2014 | Derailleurs | 167 | 41 | 10,300.16 |
| May 2014 | Derailleurs | 141 | 34 | 8,616.66 |
| Nov 2013 | Derailleurs | 35 | 11 | 1,921.29 |
| Oct 2013 | Derailleurs | 135 | 35 | 8,291.81 |
| Sep 2013 | Derailleurs | 97 | 23 | 5,972.07 |
| Apr 2014 | Fenders | 217 | 217 | 4,769.66 |
| Aug 2013 | Fenders | 151 | 151 | 3,318.98 |
| Dec 2013 | Fenders | 201 | 201 | 4,417.98 |
| Feb 2014 | Fenders | 159 | 159 | 3,494.82 |
| Jan 2014 | Fenders | 163 | 163 | 3,582.74 |
| Jul 2013 | Fenders | 153 | 153 | 3,362.94 |
| Jun 2013 | Fenders | 2 | 2 | 43.96 |
| Jun 2014 | Fenders | 103 | 103 | 2,263.94 |
| Mar 2014 | Fenders | 183 | 183 | 4,022.34 |
| May 2014 | Fenders | 208 | 208 | 4,571.84 |
| Nov 2013 | Fenders | 192 | 192 | 4,220.16 |
| Oct 2013 | Fenders | 170 | 170 | 3,736.6 |
| Sep 2013 | Fenders | 169 | 169 | 3,714.62 |
| Apr 2014 | Gloves | 155 | 148 | 3,797.58 |
| Aug 2013 | Gloves | 468 | 142 | 7,931.95 |
| Dec 2013 | Gloves | 249 | 134 | 4,834.33 |
| Feb 2014 | Gloves | 92 | 92 | 2,253.08 |
| Jan 2014 | Gloves | 250 | 151 | 5,027.45 |
| Jul 2013 | Gloves | 620 | 157 | 9,985.12 |
| Jun 2013 | Gloves | 547 | 61 | 7,946.19 |
| Jun 2014 | Gloves | 65 | 65 | 1,591.85 |
| Mar 2014 | Gloves | 643 | 196 | 10,774.55 |
| May 2014 | Gloves | 413 | 189 | 7,539.81 |
| Nov 2013 | Gloves | 265 | 137 | 5,080.51 |
| Oct 2013 | Gloves | 447 | 148 | 7,678.81 |
| Sep 2013 | Gloves | 444 | 146 | 7,552.25 |
| Aug 2013 | Handlebars | 120 | 34 | 5,992.99 |
| Dec 2013 | Handlebars | 76 | 21 | 2,521.14 |
| Jan 2014 | Handlebars | 75 | 24 | 2,483.99 |
| Jul 2013 | Handlebars | 386 | 86 | 16,878.18 |
| Jun 2013 | Handlebars | 381 | 93 | 16,508.98 |
| Mar 2014 | Handlebars | 279 | 86 | 13,237.41 |
| May 2014 | Handlebars | 178 | 62 | 8,655.01 |
| Nov 2013 | Handlebars | 40 | 12 | 1,340.09 |
| Oct 2013 | Handlebars | 188 | 56 | 9,196.1 |
| Sep 2013 | Handlebars | 131 | 47 | 6,486.59 |
| Apr 2014 | Helmets | 654 | 653 | 22,855.47 |
| Aug 2013 | Helmets | 937 | 503 | 26,093.36 |
| Dec 2013 | Helmets | 781 | 540 | 23,590.26 |
| Feb 2014 | Helmets | 517 | 516 | 18,061.84 |
| Jan 2014 | Helmets | 840 | 585 | 25,318.76 |
| Jul 2013 | Helmets | 1282 | 507 | 32,921.89 |
| Jun 2013 | Helmets | 825 | 71 | 17,218.01 |
| Jun 2014 | Helmets | 262 | 262 | 9,167.38 |
| Mar 2014 | Helmets | 1695 | 692 | 43,928.25 |
| May 2014 | Helmets | 1241 | 697 | 34,956.23 |
| Nov 2013 | Helmets | 850 | 627 | 26,270.49 |
| Oct 2013 | Helmets | 1127 | 557 | 30,733.99 |
| Sep 2013 | Helmets | 1016 | 480 | 27,354.84 |
| Apr 2014 | Hydration Packs | 75 | 75 | 4,124.25 |
| Aug 2013 | Hydration Packs | 222 | 85 | 8,536.96 |
| Dec 2013 | Hydration Packs | 139 | 78 | 5,974.8 |
| Feb 2014 | Hydration Packs | 64 | 64 | 3,519.36 |
| Jan 2014 | Hydration Packs | 141 | 90 | 6,191.87 |
| Jul 2013 | Hydration Packs | 318 | 96 | 11,419.77 |
| Jun 2013 | Hydration Packs | 319 | 58 | 10,351.78 |
| Jun 2014 | Hydration Packs | 26 | 26 | 1,429.74 |
| Mar 2014 | Hydration Packs | 368 | 101 | 13,078.8 |
| May 2014 | Hydration Packs | 285 | 107 | 10,993.09 |
| Nov 2013 | Hydration Packs | 125 | 74 | 5,400.02 |
| Oct 2013 | Hydration Packs | 243 | 86 | 9,207.61 |
| Sep 2013 | Hydration Packs | 249 | 87 | 9,186.16 |
| Apr 2014 | Jerseys | 296 | 296 | 15,357.04 |
| Aug 2013 | Jerseys | 1172 | 300 | 41,362.74 |
| Dec 2013 | Jerseys | 915 | 343 | 34,891.74 |
| Feb 2014 | Jerseys | 258 | 258 | 13,345.42 |
| Jan 2014 | Jerseys | 921 | 325 | 34,590.93 |
| Jul 2013 | Jerseys | 1890 | 283 | 61,736.06 |
| Jun 2013 | Jerseys | 1789 | 81 | 54,445.82 |
| Jun 2014 | Jerseys | 146 | 146 | 7,526.54 |
| Mar 2014 | Jerseys | 2241 | 423 | 75,862.04 |
| May 2014 | Jerseys | 1455 | 426 | 52,521.53 |
| Nov 2013 | Jerseys | 912 | 333 | 34,516.69 |
| Oct 2013 | Jerseys | 1372 | 324 | 47,761.26 |
| Sep 2013 | Jerseys | 1402 | 304 | 48,318.09 |
| Sep 2013 | Locks | 1 | 1 | 15 |
| Apr 2014 | Mountain Bikes | 404 | 404 | 745,380.46 |
| Aug 2013 | Mountain Bikes | 880 | 246 | 1,003,019.73 |
| Dec 2013 | Mountain Bikes | 1072 | 340 | 1,258,255.47 |
| Feb 2014 | Mountain Bikes | 292 | 292 | 519,976.08 |
| Jan 2014 | Mountain Bikes | 1032 | 360 | 1,257,649.7 |
| Jul 2013 | Mountain Bikes | 1295 | 214 | 1,281,345.12 |
| Jun 2013 | Mountain Bikes | 1143 | 56 | 999,504.74 |
| Mar 2014 | Mountain Bikes | 1868 | 418 | 1,922,495.8 |
| May 2014 | Mountain Bikes | 1336 | 483 | 1,578,293.99 |
| Nov 2013 | Mountain Bikes | 825 | 374 | 1,042,572.35 |
| Oct 2013 | Mountain Bikes | 1231 | 305 | 1,338,738.67 |
| Sep 2013 | Mountain Bikes | 1194 | 263 | 1,244,716.84 |
| Aug 2013 | Mountain Frames | 357 | 22 | 118,989.87 |
| Dec 2013 | Mountain Frames | 380 | 31 | 133,706.65 |
| Jan 2014 | Mountain Frames | 375 | 31 | 115,170.71 |
| Jul 2013 | Mountain Frames | 968 | 43 | 312,020.47 |
| Jun 2013 | Mountain Frames | 973 | 51 | 305,923.5 |
| Mar 2014 | Mountain Frames | 979 | 52 | 343,024.63 |
| May 2014 | Mountain Frames | 613 | 33 | 220,929.06 |
| Nov 2013 | Mountain Frames | 204 | 17 | 61,012.99 |
| Oct 2013 | Mountain Frames | 632 | 32 | 221,830.45 |
| Sep 2013 | Mountain Frames | 662 | 34 | 224,303.98 |
| Aug 2013 | Pedals | 253 | 47 | 9,274.6 |
| Dec 2013 | Pedals | 168 | 55 | 6,524.35 |
| Jan 2014 | Pedals | 145 | 46 | 5,661.03 |
| Jul 2013 | Pedals | 513 | 94 | 19,728.52 |
| Jun 2013 | Pedals | 582 | 99 | 21,697.84 |
| Mar 2014 | Pedals | 645 | 118 | 23,910.64 |
| May 2014 | Pedals | 367 | 67 | 13,728.31 |
| Nov 2013 | Pedals | 97 | 26 | 3,599.06 |
| Oct 2013 | Pedals | 374 | 72 | 14,060.98 |
| Sep 2013 | Pedals | 438 | 76 | 16,565.11 |
| Apr 2014 | Road Bikes | 443 | 443 | 527,137.55 |
| Aug 2013 | Road Bikes | 1092 | 309 | 1,031,456.98 |
| Dec 2013 | Road Bikes | 1344 | 371 | 1,284,311.56 |
| Feb 2014 | Road Bikes | 318 | 318 | 391,068.64 |
| Jan 2014 | Road Bikes | 1185 | 416 | 1,161,599.45 |
| Jul 2013 | Road Bikes | 1278 | 274 | 1,188,331.41 |
| Jun 2013 | Road Bikes | 1335 | 60 | 1,122,867.81 |
| Mar 2014 | Road Bikes | 2371 | 520 | 2,148,706.22 |
| May 2014 | Road Bikes | 1379 | 519 | 1,321,269.15 |
| Nov 2013 | Road Bikes | 1070 | 452 | 1,066,177.63 |
| Oct 2013 | Road Bikes | 1288 | 363 | 1,217,739.12 |
| Sep 2013 | Road Bikes | 1398 | 319 | 1,318,888.69 |
| Apr 2014 | Road Frames | 2 | 1 | 713.8 |
| Aug 2013 | Road Frames | 203 | 22 | 77,125.03 |
| Dec 2013 | Road Frames | 96 | 15 | 29,006.96 |
| Feb 2014 | Road Frames | 7 | 3 | 2,390.93 |
| Jan 2014 | Road Frames | 86 | 14 | 24,665.15 |
| Jul 2013 | Road Frames | 560 | 39 | 258,149.56 |
| Jun 2013 | Road Frames | 621 | 62 | 287,171.22 |
| Mar 2014 | Road Frames | 440 | 56 | 173,866.07 |
| May 2014 | Road Frames | 240 | 29 | 102,948.01 |
| Nov 2013 | Road Frames | 76 | 12 | 23,105.53 |
| Oct 2013 | Road Frames | 247 | 40 | 99,189.75 |
| Sep 2013 | Road Frames | 264 | 38 | 110,277.04 |
| Aug 2013 | Saddles | 104 | 23 | 2,687.71 |
| Dec 2013 | Saddles | 61 | 22 | 1,716.02 |
| Jan 2014 | Saddles | 75 | 21 | 2,125.8 |
| Jul 2013 | Saddles | 375 | 92 | 9,565.82 |
| Jun 2013 | Saddles | 379 | 88 | 10,038.01 |
| Mar 2014 | Saddles | 291 | 74 | 7,581.4 |
| May 2014 | Saddles | 208 | 58 | 5,416.96 |
| Nov 2013 | Saddles | 41 | 11 | 1,157.24 |
| Oct 2013 | Saddles | 226 | 56 | 5,991.37 |
| Sep 2013 | Saddles | 170 | 39 | 4,250.05 |
| Apr 2014 | Shorts | 81 | 81 | 5,669.19 |
| Aug 2013 | Shorts | 548 | 116 | 23,835.99 |
| Dec 2013 | Shorts | 512 | 131 | 23,252.37 |
| Feb 2014 | Shorts | 77 | 77 | 5,389.23 |
| Jan 2014 | Shorts | 536 | 125 | 24,469.68 |
| Jul 2013 | Shorts | 930 | 125 | 38,209.57 |
| Jun 2013 | Shorts | 867 | 47 | 33,371.76 |
| Jun 2014 | Shorts | 59 | 59 | 4,129.41 |
| Mar 2014 | Shorts | 1187 | 167 | 49,009.57 |
| May 2014 | Shorts | 680 | 145 | 30,612.36 |
| Nov 2013 | Shorts | 385 | 112 | 17,931.32 |
| Oct 2013 | Shorts | 661 | 122 | 28,533.35 |
| Sep 2013 | Shorts | 713 | 130 | 30,568.71 |
| Apr 2014 | Socks | 50 | 50 | 449.5 |
| Aug 2013 | Socks | 274 | 72 | 1,600.05 |
| Dec 2013 | Socks | 282 | 87 | 1,708.57 |
| Feb 2014 | Socks | 38 | 38 | 341.62 |
| Jan 2014 | Socks | 236 | 79 | 1,430.11 |
| Jul 2013 | Socks | 412 | 78 | 2,315.77 |
| Jun 2013 | Socks | 522 | 44 | 2,713.21 |
| Jun 2014 | Socks | 25 | 25 | 224.75 |
| Mar 2014 | Socks | 594 | 109 | 3,262.15 |
| May 2014 | Socks | 353 | 88 | 2,059.22 |
| Nov 2013 | Socks | 174 | 67 | 1,104.06 |
| Oct 2013 | Socks | 358 | 86 | 2,076.26 |
| Sep 2013 | Socks | 430 | 88 | 2,438.96 |
| Apr 2014 | Tights | 2 | 1 | 97.49 |
| Jul 2013 | Tights | 7 | 1 | 341.2 |
| Jun 2013 | Tights | 9 | 1 | 438.69 |
| Mar 2014 | Tights | 4 | 1 | 194.97 |
| Sep 2013 | Tights | 5 | 1 | 243.72 |
| Apr 2014 | Tires and Tubes | 1553 | 882 | 22,315.91 |
| Aug 2013 | Tires and Tubes | 1451 | 801 | 19,950.4 |
| Dec 2013 | Tires and Tubes | 1516 | 846 | 21,447.47 |
| Feb 2014 | Tires and Tubes | 1291 | 761 | 18,560.04 |
| Jan 2014 | Tires and Tubes | 1492 | 866 | 21,111.63 |
| Jul 2013 | Tires and Tubes | 1424 | 792 | 18,931.42 |
| Jun 2013 | Tires and Tubes | 147 | 52 | 906.96 |
| Jun 2014 | Tires and Tubes | 996 | 573 | 13,692.27 |
| Mar 2014 | Tires and Tubes | 1755 | 931 | 22,999.2 |
| May 2014 | Tires and Tubes | 1633 | 911 | 22,039.51 |
| Nov 2013 | Tires and Tubes | 1479 | 832 | 20,956.06 |
| Oct 2013 | Tires and Tubes | 1531 | 849 | 21,152.66 |
| Sep 2013 | Tires and Tubes | 1420 | 787 | 18,561.39 |
| Apr 2014 | Touring Bikes | 239 | 239 | 424,048.23 |
| Aug 2013 | Touring Bikes | 803 | 137 | 808,936.53 |
| Dec 2013 | Touring Bikes | 1009 | 229 | 1,099,947.66 |
| Feb 2014 | Touring Bikes | 189 | 189 | 343,276.95 |
| Jan 2014 | Touring Bikes | 1449 | 262 | 1,473,593.3 |
| Jul 2013 | Touring Bikes | 1683 | 150 | 1,210,589.86 |
| Jun 2013 | Touring Bikes | 1386 | 43 | 1,005,767.56 |
| Mar 2014 | Touring Bikes | 1952 | 305 | 1,973,165.63 |
| May 2014 | Touring Bikes | 1606 | 328 | 1,665,519.43 |
| Nov 2013 | Touring Bikes | 785 | 261 | 902,005.31 |
| Oct 2013 | Touring Bikes | 1522 | 221 | 1,497,460.97 |
| Sep 2013 | Touring Bikes | 1238 | 171 | 1,214,628.9 |
| Aug 2013 | Touring Frames | 165 | 9 | 75,302.78 |
| Dec 2013 | Touring Frames | 199 | 18 | 82,051.22 |
| Jan 2014 | Touring Frames | 149 | 16 | 54,347.68 |
| Jul 2013 | Touring Frames | 642 | 44 | 275,512.53 |
| Jun 2013 | Touring Frames | 573 | 33 | 237,300.01 |
| Mar 2014 | Touring Frames | 552 | 36 | 246,806.37 |
| May 2014 | Touring Frames | 373 | 41 | 181,629.6 |
| Nov 2013 | Touring Frames | 110 | 9 | 45,338.77 |
| Oct 2013 | Touring Frames | 283 | 28 | 137,878.1 |
| Sep 2013 | Touring Frames | 337 | 21 | 154,334.64 |
| Apr 2014 | Vests | 55 | 55 | 3,492.5 |
| Aug 2013 | Vests | 475 | 57 | 17,854.3 |
| Dec 2013 | Vests | 370 | 99 | 15,209.2 |
| Feb 2014 | Vests | 50 | 50 | 3,175 |
| Jan 2014 | Vests | 404 | 102 | 16,415.67 |
| Jul 2013 | Vests | 769 | 87 | 28,444.1 |
| Jun 2013 | Vests | 803 | 64 | 28,660.94 |
| Jun 2014 | Vests | 31 | 31 | 1,968.5 |
| Mar 2014 | Vests | 1051 | 149 | 40,115.27 |
| May 2014 | Vests | 610 | 103 | 23,640.71 |
| Nov 2013 | Vests | 315 | 75 | 12,937.24 |
| Oct 2013 | Vests | 611 | 93 | 23,255.74 |
| Sep 2013 | Vests | 623 | 102 | 24,100.47 |
| Jul 2013 | Wheels | 4 | 1 | 698.63 |
| Jun 2013 | Wheels | 3 | 1 | 450.91 |
| Sep 2013 | Wheels | 1 | 1 | 83.3 |

*Findings & Recommendations*
- Findings
  + **Bikes & Frames**: Highest revenue, strong volume (especially Mountain & Road)
  + **Apparel & Tires**: High quantity, steady demand, moderate value
  + **Accessories**: Low per-order value but frequent, suggesting add-on buying.
- Recommendations**
  + **Keep bikes, frames, and top apparel in stock**; bundle accessories to boost order value
  + **Run targeted marketing** on best-selling consumables and new bike launches
  + **Review slow movers** (e.g., locks, tights); apply markdowns or phase out
  + **Align purchasing/production** with high-volume items and seasonal trends


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
*Result*
| yr | TerritoryID | order_cnt | rk |
| --- | --- | --- | --- |
| 2012 | 4 | 17553 | 1 |
| 2012 | 6 | 14412 | 2 |
| 2012 | 1 | 8537 | 3 |
| 2013 | 4 | 26682 | 1 |
| 2013 | 6 | 22553 | 2 |
| 2013 | 1 | 17452 | 3 |
| 2011 | 4 | 3238 | 1 |
| 2011 | 6 | 2705 | 2 |
| 2011 | 1 | 1964 | 3 |
| 2014 | 4 | 11632 | 1 |
| 2014 | 6 | 9711 | 2 |
| 2014 | 1 | 8823 | 3 |

*Findings & Recommendations*
- Findings
  + TerritoryID 4 consistently ranks #1 in order volume from 2011 to 2014, with strong growth
  + TerritoryID 6 and 1 reliably hold 2nd and 3rd positions each year
  + These three territories dominate Adventure Works’ order volume
- Recommendations
  + Prioritize investment to maintain and expand TerritoryID 4’s market
  + Strengthen development efforts in TerritoryID 6 and 1 to capture further growth
  + Analyze success factors in these regions to replicate in other territories.

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
  + Monitor seasonal discount expenses closely to avoid excessive margin erosion
  + Evaluate the effectiveness of current discount campaigns on Helmets and consider adjusting promotion levels or channels
  + Analyze sales data alongside discount costs to ensure increased spending translates into proportional revenue growth and optimize marketing budget

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
  + Strong initial orders but steep drop in retention after month 0
  + Minor rebounds in month 3 for some cohorts
- Recommendations
  + Enhance early post-purchase engagement and loyalty efforts
  + Target incentives to improve repurchase in months 1–2

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
*Result*
| Name | mth | yr | stock_qty | stock_prv | diff |
| --- | --- | --- | --- | --- | --- |
| BB Ball Bearing | 12 | 2011 | 8475 | 14544 | -41.7 |
| BB Ball Bearing | 11 | 2011 | 14544 | 19175 | -24.2 |
| BB Ball Bearing | 10 | 2011 | 19175 | 8845 | 116.8 |
| BB Ball Bearing | 9 | 2011 | 8845 | 9666 | -8.5 |
| BB Ball Bearing | 8 | 2011 | 9666 | 12837 | -24.7 |
| BB Ball Bearing | 7 | 2011 | 12837 | 5259 | 144.1 |
| BB Ball Bearing | 6 | 2011 | 5259 |  | 0 |
| Blade | 12 | 2011 | 1842 | 3598 | -48.8 |
| Blade | 11 | 2011 | 3598 | 4670 | -23 |
| Blade | 10 | 2011 | 4670 | 2122 | 120.1 |
| Blade | 9 | 2011 | 2122 | 2382 | -10.9 |
| Blade | 8 | 2011 | 2382 | 3166 | -24.8 |
| Blade | 7 | 2011 | 3166 | 1280 | 147.3 |
| Blade | 6 | 2011 | 1280 |  | 0 |
| Chain Stays | 12 | 2011 | 1842 | 3598 | -48.8 |
| Chain Stays | 11 | 2011 | 3598 | 4670 | -23 |
| Chain Stays | 10 | 2011 | 4670 | 2122 | 120.1 |
| Chain Stays | 9 | 2011 | 2122 | 2341 | -9.4 |
| Chain Stays | 8 | 2011 | 2341 | 3166 | -26.1 |
| Chain Stays | 7 | 2011 | 3166 | 1280 | 147.3 |
| Chain Stays | 6 | 2011 | 1280 |  | 0 |
| Down Tube | 12 | 2011 | 921 | 1799 | -48.8 |
| Down Tube | 11 | 2011 | 1799 | 2335 | -23 |
| Down Tube | 10 | 2011 | 2335 | 1061 | 120.1 |
| Down Tube | 9 | 2011 | 1061 | 1191 | -10.9 |
| Down Tube | 8 | 2011 | 1191 | 1541 | -22.7 |
| Down Tube | 7 | 2011 | 1541 | 640 | 140.8 |
| Down Tube | 6 | 2011 | 640 |  | 0 |
| Fork Crown | 12 | 2011 | 921 | 1799 | -48.8 |
| Fork Crown | 11 | 2011 | 1799 | 2335 | -23 |
| Fork Crown | 10 | 2011 | 2335 | 1061 | 120.1 |
| Fork Crown | 9 | 2011 | 1061 | 1191 | -10.9 |
| Fork Crown | 8 | 2011 | 1191 | 1583 | -24.8 |
| Fork Crown | 7 | 2011 | 1583 | 640 | 147.3 |
| Fork Crown | 6 | 2011 | 640 |  | 0 |
| Fork End | 12 | 2011 | 1842 | 3598 | -48.8 |
| Fork End | 11 | 2011 | 3598 | 4670 | -23 |
| Fork End | 10 | 2011 | 4670 | 2122 | 120.1 |
| Fork End | 9 | 2011 | 2122 | 2382 | -10.9 |
| Fork End | 8 | 2011 | 2382 | 3166 | -24.8 |
| Fork End | 7 | 2011 | 3166 | 1280 | 147.3 |
| Fork End | 6 | 2011 | 1280 |  | 0 |
| Front Derailleur | 12 | 2011 | 861 | 1440 | -40.2 |
| Front Derailleur | 11 | 2011 | 1440 | 1918 | -24.9 |
| Front Derailleur | 10 | 2011 | 1918 | 874 | 119.5 |
| Front Derailleur | 9 | 2011 | 874 | 969 | -9.8 |
| Front Derailleur | 8 | 2011 | 969 | 1257 | -22.9 |
| Front Derailleur | 7 | 2011 | 1257 | 501 | 150.9 |
| Front Derailleur | 6 | 2011 | 501 |  | 0 |
| HL Bottom Bracket | 12 | 2011 | 426 | 719 | -40.8 |
| HL Bottom Bracket | 11 | 2011 | 719 | 938 | -23.3 |
| HL Bottom Bracket | 10 | 2011 | 938 | 395 | 137.5 |
| HL Bottom Bracket | 9 | 2011 | 395 | 534 | -26 |
| HL Bottom Bracket | 8 | 2011 | 534 | 670 | -20.3 |
| HL Bottom Bracket | 7 | 2011 | 670 | 176 | 280.7 |
| HL Bottom Bracket | 6 | 2011 | 176 |  | 0 |
| HL Crankset | 12 | 2011 | 426 | 719 | -40.8 |
| HL Crankset | 11 | 2011 | 719 | 938 | -23.3 |
| HL Crankset | 10 | 2011 | 938 | 395 | 137.5 |
| HL Crankset | 9 | 2011 | 395 | 548 | -27.9 |
| HL Crankset | 8 | 2011 | 548 | 670 | -18.2 |
| HL Crankset | 7 | 2011 | 670 | 176 | 280.7 |
| HL Crankset | 6 | 2011 | 176 |  | 0 |
| HL Fork | 12 | 2011 | 441 | 856 | -48.5 |
| HL Fork | 11 | 2011 | 856 | 1064 | -19.5 |
| HL Fork | 10 | 2011 | 1064 | 441 | 141.3 |
| HL Fork | 9 | 2011 | 441 | 650 | -32.2 |
| HL Fork | 8 | 2011 | 650 | 753 | -13.7 |
| HL Fork | 7 | 2011 | 753 | 189 | 298.4 |
| HL Fork | 6 | 2011 | 189 |  | 0 |
| HL Headset | 12 | 2011 | 426 | 719 | -40.8 |
| HL Headset | 11 | 2011 | 719 | 938 | -23.3 |
| HL Headset | 10 | 2011 | 938 | 395 | 137.5 |
| HL Headset | 9 | 2011 | 395 | 548 | -27.9 |
| HL Headset | 8 | 2011 | 548 | 670 | -18.2 |
| HL Headset | 7 | 2011 | 670 | 176 | 280.7 |
| HL Headset | 6 | 2011 | 176 |  | 0 |
| HL Hub | 12 | 2011 | 1044 | 1886 | -44.6 |
| HL Hub | 11 | 2011 | 1886 | 2454 | -23.1 |
| HL Hub | 10 | 2011 | 2454 | 1032 | 137.8 |
| HL Hub | 9 | 2011 | 1032 | 1346 | -23.3 |
| HL Hub | 8 | 2011 | 1346 | 1648 | -18.3 |
| HL Hub | 7 | 2011 | 1648 | 594 | 177.4 |
| HL Hub | 6 | 2011 | 594 |  | 0 |
| HL Mountain Frame - Black, 38 | 12 | 2011 | 31 | 90 | -65.6 |
| HL Mountain Frame - Black, 38 | 11 | 2011 | 90 | 94 | -4.3 |
| HL Mountain Frame - Black, 38 | 10 | 2011 | 94 | 34 | 176.5 |
| HL Mountain Frame - Black, 38 | 9 | 2011 | 34 | 69 | -50.7 |
| HL Mountain Frame - Black, 38 | 8 | 2011 | 69 | 85 | -18.8 |
| HL Mountain Frame - Black, 38 | 7 | 2011 | 85 | 26 | 226.9 |
| HL Mountain Frame - Black, 38 | 6 | 2011 | 26 |  | 0 |
| HL Mountain Frame - Black, 42 | 12 | 2011 | 26 | 90 | -71.1 |
| HL Mountain Frame - Black, 42 | 11 | 2011 | 90 | 96 | -6.3 |
| HL Mountain Frame - Black, 42 | 10 | 2011 | 96 | 40 | 140 |
| HL Mountain Frame - Black, 42 | 9 | 2011 | 40 | 64 | -37.5 |
| HL Mountain Frame - Black, 42 | 8 | 2011 | 64 | 91 | -29.7 |
| HL Mountain Frame - Black, 42 | 7 | 2011 | 91 | 17 | 435.3 |
| HL Mountain Frame - Black, 42 | 6 | 2011 | 17 |  | 0 |
| HL Mountain Frame - Black, 44 | 12 | 2011 | 29 | 72 | -59.7 |
| HL Mountain Frame - Black, 44 | 11 | 2011 | 72 | 100 | -28 |
| HL Mountain Frame - Black, 44 | 10 | 2011 | 100 | 28 | 257.1 |
| HL Mountain Frame - Black, 44 | 9 | 2011 | 28 | 60 | -53.3 |
| HL Mountain Frame - Black, 44 | 8 | 2011 | 60 | 64 | -6.3 |
| HL Mountain Frame - Black, 44 | 7 | 2011 | 64 | 26 | 146.2 |
| HL Mountain Frame - Black, 44 | 6 | 2011 | 26 |  | 0 |
| HL Mountain Frame - Black, 48 | 12 | 2011 | 27 | 76 | -64.5 |
| HL Mountain Frame - Black, 48 | 11 | 2011 | 76 | 96 | -20.8 |
| HL Mountain Frame - Black, 48 | 10 | 2011 | 96 | 26 | 269.2 |
| HL Mountain Frame - Black, 48 | 9 | 2011 | 26 | 61 | -57.4 |
| HL Mountain Frame - Black, 48 | 8 | 2011 | 61 | 83 | -26.5 |
| HL Mountain Frame - Black, 48 | 7 | 2011 | 83 | 22 | 277.3 |
| HL Mountain Frame - Black, 48 | 6 | 2011 | 22 |  | 0 |
| HL Mountain Frame - Silver, 38 | 12 | 2011 | 32 | 77 | -58.4 |
| HL Mountain Frame - Silver, 38 | 11 | 2011 | 77 | 112 | -31.3 |
| HL Mountain Frame - Silver, 38 | 10 | 2011 | 112 | 33 | 239.4 |
| HL Mountain Frame - Silver, 38 | 9 | 2011 | 33 | 73 | -54.8 |
| HL Mountain Frame - Silver, 38 | 8 | 2011 | 73 | 96 | -24 |
| HL Mountain Frame - Silver, 38 | 7 | 2011 | 96 | 13 | 638.5 |
| HL Mountain Frame - Silver, 38 | 6 | 2011 | 13 |  | 0 |
| HL Mountain Frame - Silver, 42 | 12 | 2011 | 16 | 52 | -69.2 |
| HL Mountain Frame - Silver, 42 | 11 | 2011 | 52 | 97 | -46.4 |
| HL Mountain Frame - Silver, 42 | 10 | 2011 | 97 | 29 | 234.5 |
| HL Mountain Frame - Silver, 42 | 9 | 2011 | 29 | 49 | -40.8 |
| HL Mountain Frame - Silver, 42 | 8 | 2011 | 49 | 59 | -16.9 |
| HL Mountain Frame - Silver, 42 | 7 | 2011 | 59 | 5 | 1,08 |
| HL Mountain Frame - Silver, 42 | 6 | 2011 | 5 |  | 0 |
| HL Mountain Frame - Silver, 44 | 12 | 2011 | 33 | 65 | -49.2 |
| HL Mountain Frame - Silver, 44 | 11 | 2011 | 65 | 71 | -8.5 |
| HL Mountain Frame - Silver, 44 | 10 | 2011 | 71 | 24 | 195.8 |
| HL Mountain Frame - Silver, 44 | 9 | 2011 | 24 | 50 | -52 |
| HL Mountain Frame - Silver, 44 | 8 | 2011 | 50 | 59 | -15.3 |
| HL Mountain Frame - Silver, 44 | 7 | 2011 | 59 | 21 | 181 |
| HL Mountain Frame - Silver, 44 | 6 | 2011 | 21 |  | 0 |
| HL Mountain Frame - Silver, 46 | 12 | 2011 | 3 | 15 | -80 |
| HL Mountain Frame - Silver, 46 | 11 | 2011 | 15 | 17 | -11.8 |
| HL Mountain Frame - Silver, 46 | 10 | 2011 | 17 | 3 | 466.7 |
| HL Mountain Frame - Silver, 46 | 9 | 2011 | 3 | 18 | -83.3 |
| HL Mountain Frame - Silver, 46 | 8 | 2011 | 18 | 15 | 20 |
| HL Mountain Frame - Silver, 46 | 7 | 2011 | 15 | 3 | 400 |
| HL Mountain Frame - Silver, 46 | 6 | 2011 | 3 |  | 0 |
| HL Mountain Frame - Silver, 48 | 12 | 2011 | 27 | 78 | -65.4 |
| HL Mountain Frame - Silver, 48 | 11 | 2011 | 78 | 90 | -13.3 |
| HL Mountain Frame - Silver, 48 | 10 | 2011 | 90 | 30 | 200 |
| HL Mountain Frame - Silver, 48 | 9 | 2011 | 30 | 57 | -47.4 |
| HL Mountain Frame - Silver, 48 | 8 | 2011 | 57 | 58 | -1.7 |
| HL Mountain Frame - Silver, 48 | 7 | 2011 | 58 | 10 | 480 |
| HL Mountain Frame - Silver, 48 | 6 | 2011 | 10 |  | 0 |
| HL Mountain Front Wheel | 12 | 2011 | 209 | 483 | -56.7 |
| HL Mountain Front Wheel | 11 | 2011 | 483 | 659 | -26.7 |
| HL Mountain Front Wheel | 10 | 2011 | 659 | 208 | 216.8 |
| HL Mountain Front Wheel | 9 | 2011 | 208 | 396 | -47.5 |
| HL Mountain Front Wheel | 8 | 2011 | 396 | 520 | -23.8 |
| HL Mountain Front Wheel | 7 | 2011 | 520 | 124 | 319.4 |
| HL Mountain Front Wheel | 6 | 2011 | 124 |  | 0 |
| HL Mountain Handlebars | 12 | 2011 | 204 | 483 | -57.8 |
| HL Mountain Handlebars | 11 | 2011 | 483 | 659 | -26.7 |
| HL Mountain Handlebars | 10 | 2011 | 659 | 203 | 224.6 |
| HL Mountain Handlebars | 9 | 2011 | 203 | 396 | -48.7 |
| HL Mountain Handlebars | 8 | 2011 | 396 | 520 | -23.8 |
| HL Mountain Handlebars | 7 | 2011 | 520 | 121 | 329.8 |
| HL Mountain Handlebars | 6 | 2011 | 121 |  | 0 |
| HL Mountain Rear Wheel | 12 | 2011 | 209 | 483 | -56.7 |
| HL Mountain Rear Wheel | 11 | 2011 | 483 | 659 | -26.7 |
| HL Mountain Rear Wheel | 10 | 2011 | 659 | 208 | 216.8 |
| HL Mountain Rear Wheel | 9 | 2011 | 208 | 396 | -47.5 |
| HL Mountain Rear Wheel | 8 | 2011 | 396 | 520 | -23.8 |
| HL Mountain Rear Wheel | 7 | 2011 | 520 | 124 | 319.4 |
| HL Mountain Rear Wheel | 6 | 2011 | 124 |  | 0 |
| HL Mountain Seat Assembly | 12 | 2011 | 204 | 483 | -57.8 |
| HL Mountain Seat Assembly | 11 | 2011 | 483 | 659 | -26.7 |
| HL Mountain Seat Assembly | 10 | 2011 | 659 | 202 | 226.2 |
| HL Mountain Seat Assembly | 9 | 2011 | 202 | 396 | -49 |
| HL Mountain Seat Assembly | 8 | 2011 | 396 | 520 | -23.8 |
| HL Mountain Seat Assembly | 7 | 2011 | 520 | 124 | 319.4 |
| HL Mountain Seat Assembly | 6 | 2011 | 124 |  | 0 |
| HL Road Frame - Red, 44 | 12 | 2011 | 33 | 53 | -37.7 |
| HL Road Frame - Red, 44 | 11 | 2011 | 53 | 41 | 29.3 |
| HL Road Frame - Red, 44 | 10 | 2011 | 41 | 42 | -2.4 |
| HL Road Frame - Red, 44 | 9 | 2011 | 42 | 19 | 121.1 |
| HL Road Frame - Red, 44 | 8 | 2011 | 19 | 18 | 5.6 |
| HL Road Frame - Red, 44 | 7 | 2011 | 18 | 14 | 28.6 |
| HL Road Frame - Red, 44 | 6 | 2011 | 14 |  | 0 |
| HL Road Frame - Red, 48 | 12 | 2011 | 41 | 47 | -12.8 |
| HL Road Frame - Red, 48 | 11 | 2011 | 47 | 45 | 4.4 |
| HL Road Frame - Red, 48 | 10 | 2011 | 45 | 36 | 25 |
| HL Road Frame - Red, 48 | 9 | 2011 | 36 | 18 | 100 |
| HL Road Frame - Red, 48 | 8 | 2011 | 18 | 32 | -43.8 |
| HL Road Frame - Red, 48 | 7 | 2011 | 32 | 10 | 220 |
| HL Road Frame - Red, 48 | 6 | 2011 | 10 |  | 0 |
| HL Road Frame - Red, 52 | 12 | 2011 | 33 | 43 | -23.3 |
| HL Road Frame - Red, 52 | 11 | 2011 | 43 | 54 | -20.4 |
| HL Road Frame - Red, 52 | 10 | 2011 | 54 | 28 | 92.9 |
| HL Road Frame - Red, 52 | 9 | 2011 | 28 | 26 | 7.7 |
| HL Road Frame - Red, 52 | 8 | 2011 | 26 | 15 | 73.3 |
| HL Road Frame - Red, 52 | 7 | 2011 | 15 | 7 | 114.3 |
| HL Road Frame - Red, 52 | 6 | 2011 | 7 |  | 0 |
| HL Road Frame - Red, 56 | 12 | 2011 | 44 | 56 | -21.4 |
| HL Road Frame - Red, 56 | 11 | 2011 | 56 | 78 | -28.2 |
| HL Road Frame - Red, 56 | 10 | 2011 | 78 | 44 | 77.3 |
| HL Road Frame - Red, 56 | 9 | 2011 | 44 | 44 | 0 |
| HL Road Frame - Red, 56 | 8 | 2011 | 44 | 57 | -22.8 |
| HL Road Frame - Red, 56 | 7 | 2011 | 57 | 23 | 147.8 |
| HL Road Frame - Red, 56 | 6 | 2011 | 23 |  | 0 |
| HL Road Frame - Red, 62 | 12 | 2011 | 51 | 59 | -13.6 |
| HL Road Frame - Red, 62 | 11 | 2011 | 59 | 66 | -10.6 |
| HL Road Frame - Red, 62 | 10 | 2011 | 66 | 55 | 20 |
| HL Road Frame - Red, 62 | 9 | 2011 | 55 | 42 | 31 |
| HL Road Frame - Red, 62 | 8 | 2011 | 42 | 34 | 23.5 |
| HL Road Frame - Red, 62 | 7 | 2011 | 34 | 15 | 126.7 |
| HL Road Frame - Red, 62 | 6 | 2011 | 15 |  | 0 |
| HL Road Front Wheel | 12 | 2011 | 217 | 236 | -8.1 |
| HL Road Front Wheel | 11 | 2011 | 236 | 279 | -15.4 |
| HL Road Front Wheel | 10 | 2011 | 279 | 187 | 49.2 |
| HL Road Front Wheel | 9 | 2011 | 187 | 152 | 23 |
| HL Road Front Wheel | 8 | 2011 | 152 | 150 | 1.3 |
| HL Road Front Wheel | 7 | 2011 | 150 | 52 | 188.5 |
| HL Road Front Wheel | 6 | 2011 | 52 |  | 0 |
| HL Road Handlebars | 12 | 2011 | 217 | 236 | -8.1 |
| HL Road Handlebars | 11 | 2011 | 236 | 279 | -15.4 |
| HL Road Handlebars | 10 | 2011 | 279 | 187 | 49.2 |
| HL Road Handlebars | 9 | 2011 | 187 | 152 | 23 |
| HL Road Handlebars | 8 | 2011 | 152 | 150 | 1.3 |
| HL Road Handlebars | 7 | 2011 | 150 | 52 | 188.5 |
| HL Road Handlebars | 6 | 2011 | 52 |  | 0 |
| HL Road Rear Wheel | 12 | 2011 | 217 | 236 | -8.1 |
| HL Road Rear Wheel | 11 | 2011 | 236 | 276 | -14.5 |
| HL Road Rear Wheel | 10 | 2011 | 276 | 187 | 47.6 |
| HL Road Rear Wheel | 9 | 2011 | 187 | 152 | 23 |
| HL Road Rear Wheel | 8 | 2011 | 152 | 150 | 1.3 |
| HL Road Rear Wheel | 7 | 2011 | 150 | 52 | 188.5 |
| HL Road Rear Wheel | 6 | 2011 | 52 |  | 0 |
| HL Road Seat Assembly | 12 | 2011 | 217 | 236 | -8.1 |
| HL Road Seat Assembly | 11 | 2011 | 236 | 279 | -15.4 |
| HL Road Seat Assembly | 10 | 2011 | 279 | 187 | 49.2 |
| HL Road Seat Assembly | 9 | 2011 | 187 | 152 | 23 |
| HL Road Seat Assembly | 8 | 2011 | 152 | 150 | 1.3 |
| HL Road Seat Assembly | 7 | 2011 | 150 | 52 | 188.5 |
| HL Road Seat Assembly | 6 | 2011 | 52 |  | 0 |
| Handlebar Tube | 12 | 2011 | 848 | 1455 | -41.7 |
| Handlebar Tube | 11 | 2011 | 1455 | 1918 | -24.1 |
| Handlebar Tube | 10 | 2011 | 1918 | 885 | 116.7 |
| Handlebar Tube | 9 | 2011 | 885 | 967 | -8.5 |
| Handlebar Tube | 8 | 2011 | 967 | 1284 | -24.7 |
| Handlebar Tube | 7 | 2011 | 1284 | 526 | 144.1 |
| Handlebar Tube | 6 | 2011 | 526 |  | 0 |
| Head Tube | 12 | 2011 | 921 | 1799 | -48.8 |
| Head Tube | 11 | 2011 | 1799 | 2335 | -23 |
| Head Tube | 10 | 2011 | 2335 | 1061 | 120.1 |
| Head Tube | 9 | 2011 | 1061 | 1165 | -8.9 |
| Head Tube | 8 | 2011 | 1165 | 1583 | -26.4 |
| Head Tube | 7 | 2011 | 1583 | 640 | 147.3 |
| Head Tube | 6 | 2011 | 640 |  | 0 |
| LL Bottom Bracket | 12 | 2011 | 316 | 514 | -38.5 |
| LL Bottom Bracket | 11 | 2011 | 514 | 689 | -25.4 |
| LL Bottom Bracket | 10 | 2011 | 689 | 362 | 90.3 |
| LL Bottom Bracket | 9 | 2011 | 362 | 293 | 23.5 |
| LL Bottom Bracket | 8 | 2011 | 293 | 439 | -33.3 |
| LL Bottom Bracket | 7 | 2011 | 439 | 223 | 96.9 |
| LL Bottom Bracket | 6 | 2011 | 223 |  | 0 |
| LL Fork | 12 | 2011 | 370 | 679 | -45.5 |
| LL Fork | 11 | 2011 | 679 | 926 | -26.7 |
| LL Fork | 10 | 2011 | 926 | 479 | 93.3 |
| LL Fork | 9 | 2011 | 479 | 383 | 25.1 |
| LL Fork | 8 | 2011 | 383 | 609 | -37.1 |
| LL Fork | 7 | 2011 | 609 | 308 | 97.7 |
| LL Fork | 6 | 2011 | 308 |  | 0 |
| LL Hub | 12 | 2011 | 652 | 1024 | -36.3 |
| LL Hub | 11 | 2011 | 1024 | 1382 | -25.9 |
| LL Hub | 10 | 2011 | 1382 | 738 | 87.3 |
| LL Hub | 9 | 2011 | 738 | 588 | 25.5 |
| LL Hub | 8 | 2011 | 588 | 878 | -33 |
| LL Hub | 7 | 2011 | 878 | 458 | 91.7 |
| LL Hub | 6 | 2011 | 458 |  | 0 |
| LL Road Frame - Black, 44 | 12 | 2011 | 21 | 33 | -36.4 |
| LL Road Frame - Black, 44 | 11 | 2011 | 33 | 45 | -26.7 |
| LL Road Frame - Black, 44 | 10 | 2011 | 45 | 30 | 50 |
| LL Road Frame - Black, 44 | 9 | 2011 | 30 | 18 | 66.7 |
| LL Road Frame - Black, 44 | 8 | 2011 | 18 | 19 | -5.3 |
| LL Road Frame - Black, 44 | 7 | 2011 | 19 | 13 | 46.2 |
| LL Road Frame - Black, 44 | 6 | 2011 | 13 |  | 0 |
| LL Road Frame - Black, 48 | 12 | 2011 | 8 | 15 | -46.7 |
| LL Road Frame - Black, 48 | 11 | 2011 | 15 | 30 | -50 |
| LL Road Frame - Black, 48 | 10 | 2011 | 30 | 19 | 57.9 |
| LL Road Frame - Black, 48 | 9 | 2011 | 19 |  | 0 |
| LL Road Frame - Black, 52 | 12 | 2011 | 64 | 82 | -22 |
| LL Road Frame - Black, 52 | 11 | 2011 | 82 | 133 | -38.3 |
| LL Road Frame - Black, 52 | 10 | 2011 | 133 | 71 | 87.3 |
| LL Road Frame - Black, 52 | 9 | 2011 | 71 | 71 | 0 |
| LL Road Frame - Black, 52 | 8 | 2011 | 71 | 126 | -43.7 |
| LL Road Frame - Black, 52 | 7 | 2011 | 126 | 48 | 162.5 |
| LL Road Frame - Black, 52 | 6 | 2011 | 48 |  | 0 |
| LL Road Frame - Black, 58 | 12 | 2011 | 47 | 68 | -30.9 |
| LL Road Frame - Black, 58 | 11 | 2011 | 68 | 107 | -36.4 |
| LL Road Frame - Black, 58 | 10 | 2011 | 107 | 59 | 81.4 |
| LL Road Frame - Black, 58 | 9 | 2011 | 59 | 34 | 73.5 |
| LL Road Frame - Black, 58 | 8 | 2011 | 34 | 67 | -49.3 |
| LL Road Frame - Black, 58 | 7 | 2011 | 67 | 41 | 63.4 |
| LL Road Frame - Black, 58 | 6 | 2011 | 41 |  | 0 |
| LL Road Frame - Black, 60 | 12 | 2011 | 20 | 25 | -20 |
| LL Road Frame - Black, 60 | 11 | 2011 | 25 | 46 | -45.7 |
| LL Road Frame - Black, 60 | 10 | 2011 | 46 | 31 | 48.4 |
| LL Road Frame - Black, 60 | 9 | 2011 | 31 | 19 | 63.2 |
| LL Road Frame - Black, 60 | 8 | 2011 | 19 | 16 | 18.8 |
| LL Road Frame - Black, 60 | 7 | 2011 | 16 | 11 | 45.5 |
| LL Road Frame - Black, 60 | 6 | 2011 | 11 |  | 0 |
| LL Road Frame - Black, 62 | 12 | 2011 | 9 | 19 | -52.6 |
| LL Road Frame - Black, 62 | 11 | 2011 | 19 | 37 | -48.6 |
| LL Road Frame - Black, 62 | 10 | 2011 | 37 | 17 | 117.6 |
| LL Road Frame - Black, 62 | 9 | 2011 | 17 | 2 | 750 |
| LL Road Frame - Black, 62 | 8 | 2011 | 2 | 1 | 100 |
| LL Road Frame - Black, 62 | 7 | 2011 | 1 | 2 | -50 |
| LL Road Frame - Black, 62 | 6 | 2011 | 2 |  | 0 |
| LL Road Frame - Red, 44 | 12 | 2011 | 53 | 118 | -55.1 |
| LL Road Frame - Red, 44 | 11 | 2011 | 118 | 106 | 11.3 |
| LL Road Frame - Red, 44 | 10 | 2011 | 106 | 62 | 71 |
| LL Road Frame - Red, 44 | 9 | 2011 | 62 | 78 | -20.5 |
| LL Road Frame - Red, 44 | 8 | 2011 | 78 | 104 | -25 |
| LL Road Frame - Red, 44 | 7 | 2011 | 104 | 59 | 76.3 |
| LL Road Frame - Red, 44 | 6 | 2011 | 59 |  | 0 |
| LL Road Frame - Red, 48 | 12 | 2011 | 36 | 78 | -53.8 |
| LL Road Frame - Red, 48 | 11 | 2011 | 78 | 107 | -27.1 |
| LL Road Frame - Red, 48 | 10 | 2011 | 107 | 53 | 101.9 |
| LL Road Frame - Red, 48 | 9 | 2011 | 53 | 41 | 29.3 |
| LL Road Frame - Red, 48 | 8 | 2011 | 41 | 73 | -43.8 |
| LL Road Frame - Red, 48 | 7 | 2011 | 73 | 29 | 151.7 |
| LL Road Frame - Red, 48 | 6 | 2011 | 29 |  | 0 |
| LL Road Frame - Red, 52 | 12 | 2011 | 23 | 33 | -30.3 |
| LL Road Frame - Red, 52 | 11 | 2011 | 33 | 50 | -34 |
| LL Road Frame - Red, 52 | 10 | 2011 | 50 | 23 | 117.4 |
| LL Road Frame - Red, 52 | 9 | 2011 | 23 | 14 | 64.3 |
| LL Road Frame - Red, 52 | 8 | 2011 | 14 | 16 | -12.5 |
| LL Road Frame - Red, 52 | 7 | 2011 | 16 | 15 | 6.7 |
| LL Road Frame - Red, 52 | 6 | 2011 | 15 |  | 0 |
| LL Road Frame - Red, 58 | 12 | 2011 | 12 | 20 | -40 |
| LL Road Frame - Red, 58 | 11 | 2011 | 20 | 33 | -39.4 |
| LL Road Frame - Red, 58 | 10 | 2011 | 33 | 7 | 371.4 |
| LL Road Frame - Red, 58 | 9 | 2011 | 7 | 1 | 600 |
| LL Road Frame - Red, 58 | 8 | 2011 | 1 |  | 0 |
| LL Road Frame - Red, 60 | 12 | 2011 | 43 | 98 | -56.1 |
| LL Road Frame - Red, 60 | 11 | 2011 | 98 | 126 | -22.2 |
| LL Road Frame - Red, 60 | 10 | 2011 | 126 | 58 | 117.2 |
| LL Road Frame - Red, 60 | 9 | 2011 | 58 | 72 | -19.4 |
| LL Road Frame - Red, 60 | 8 | 2011 | 72 | 112 | -35.7 |
| LL Road Frame - Red, 60 | 7 | 2011 | 112 | 59 | 89.8 |
| LL Road Frame - Red, 60 | 6 | 2011 | 59 |  | 0 |
| LL Road Frame - Red, 62 | 12 | 2011 | 38 | 87 | -56.3 |
| LL Road Frame - Red, 62 | 11 | 2011 | 87 | 105 | -17.1 |
| LL Road Frame - Red, 62 | 10 | 2011 | 105 | 49 | 114.3 |
| LL Road Frame - Red, 62 | 9 | 2011 | 49 | 44 | 11.4 |
| LL Road Frame - Red, 62 | 8 | 2011 | 44 | 75 | -41.3 |
| LL Road Frame - Red, 62 | 7 | 2011 | 75 | 33 | 127.3 |
| LL Road Frame - Red, 62 | 6 | 2011 | 33 |  | 0 |
| LL Road Front Wheel | 12 | 2011 | 322 | 514 | -37.4 |
| LL Road Front Wheel | 11 | 2011 | 514 | 689 | -25.4 |
| LL Road Front Wheel | 10 | 2011 | 689 | 369 | 86.7 |
| LL Road Front Wheel | 9 | 2011 | 369 | 293 | 25.9 |
| LL Road Front Wheel | 8 | 2011 | 293 | 439 | -33.3 |
| LL Road Front Wheel | 7 | 2011 | 439 | 227 | 93.4 |
| LL Road Front Wheel | 6 | 2011 | 227 |  | 0 |
| LL Road Handlebars | 12 | 2011 | 322 | 514 | -37.4 |
| LL Road Handlebars | 11 | 2011 | 514 | 689 | -25.4 |
| LL Road Handlebars | 10 | 2011 | 689 | 369 | 86.7 |
| LL Road Handlebars | 9 | 2011 | 369 | 293 | 25.9 |
| LL Road Handlebars | 8 | 2011 | 293 | 439 | -33.3 |
| LL Road Handlebars | 7 | 2011 | 439 | 227 | 93.4 |
| LL Road Handlebars | 6 | 2011 | 227 |  | 0 |
| LL Road Rear Wheel | 12 | 2011 | 322 | 497 | -35.2 |
| LL Road Rear Wheel | 11 | 2011 | 497 | 689 | -27.9 |
| LL Road Rear Wheel | 10 | 2011 | 689 | 369 | 86.7 |
| LL Road Rear Wheel | 9 | 2011 | 369 | 293 | 25.9 |
| LL Road Rear Wheel | 8 | 2011 | 293 | 439 | -33.3 |
| LL Road Rear Wheel | 7 | 2011 | 439 | 227 | 93.4 |
| LL Road Rear Wheel | 6 | 2011 | 227 |  | 0 |
| LL Road Seat Assembly | 12 | 2011 | 322 | 514 | -37.4 |
| LL Road Seat Assembly | 11 | 2011 | 514 | 689 | -25.4 |
| LL Road Seat Assembly | 10 | 2011 | 689 | 369 | 86.7 |
| LL Road Seat Assembly | 9 | 2011 | 369 | 293 | 25.9 |
| LL Road Seat Assembly | 8 | 2011 | 293 | 427 | -31.4 |
| LL Road Seat Assembly | 7 | 2011 | 427 | 227 | 88.1 |
| LL Road Seat Assembly | 6 | 2011 | 227 |  | 0 |
| ML Bottom Bracket | 12 | 2011 | 113 | 207 | -45.4 |
| ML Bottom Bracket | 11 | 2011 | 207 | 291 | -28.9 |
| ML Bottom Bracket | 10 | 2011 | 291 | 110 | 164.5 |
| ML Bottom Bracket | 9 | 2011 | 110 | 128 | -14.1 |
| ML Bottom Bracket | 8 | 2011 | 128 | 169 | -24.3 |
| ML Bottom Bracket | 7 | 2011 | 169 | 98 | 72.4 |
| ML Bottom Bracket | 6 | 2011 | 98 |  | 0 |
| ML Crankset | 12 | 2011 | 435 | 721 | -39.7 |
| ML Crankset | 11 | 2011 | 721 | 980 | -26.4 |
| ML Crankset | 10 | 2011 | 980 | 479 | 104.6 |
| ML Crankset | 9 | 2011 | 479 | 409 | 17.1 |
| ML Crankset | 8 | 2011 | 409 | 608 | -32.7 |
| ML Crankset | 7 | 2011 | 608 | 325 | 87.1 |
| ML Crankset | 6 | 2011 | 325 |  | 0 |
| ML Fork | 12 | 2011 | 123 | 249 | -50.6 |
| ML Fork | 11 | 2011 | 249 | 345 | -27.8 |
| ML Fork | 10 | 2011 | 345 | 130 | 165.4 |
| ML Fork | 9 | 2011 | 130 | 150 | -13.3 |
| ML Fork | 8 | 2011 | 150 | 203 | -26.1 |
| ML Fork | 7 | 2011 | 203 | 118 | 72 |
| ML Fork | 6 | 2011 | 118 |  | 0 |
| ML Headset | 12 | 2011 | 435 | 721 | -39.7 |
| ML Headset | 11 | 2011 | 721 | 980 | -26.4 |
| ML Headset | 10 | 2011 | 980 | 479 | 104.6 |
| ML Headset | 9 | 2011 | 479 | 407 | 17.7 |
| ML Headset | 8 | 2011 | 407 | 608 | -33.1 |
| ML Headset | 7 | 2011 | 608 | 325 | 87.1 |
| ML Headset | 6 | 2011 | 325 |  | 0 |
| ML Road Frame - Red, 44 | 12 | 2011 | 23 | 36 | -36.1 |
| ML Road Frame - Red, 44 | 11 | 2011 | 36 | 46 | -21.7 |
| ML Road Frame - Red, 44 | 10 | 2011 | 46 | 19 | 142.1 |
| ML Road Frame - Red, 44 | 9 | 2011 | 19 | 21 | -9.5 |
| ML Road Frame - Red, 44 | 8 | 2011 | 21 | 21 | 0 |
| ML Road Frame - Red, 44 | 7 | 2011 | 21 | 14 | 50 |
| ML Road Frame - Red, 44 | 6 | 2011 | 14 |  | 0 |
| ML Road Frame - Red, 48 | 12 | 2011 | 16 | 45 | -64.4 |
| ML Road Frame - Red, 48 | 11 | 2011 | 45 | 68 | -33.8 |
| ML Road Frame - Red, 48 | 10 | 2011 | 68 | 20 | 240 |
| ML Road Frame - Red, 48 | 9 | 2011 | 20 | 14 | 42.9 |
| ML Road Frame - Red, 48 | 8 | 2011 | 14 | 26 | -46.2 |
| ML Road Frame - Red, 48 | 7 | 2011 | 26 | 16 | 62.5 |
| ML Road Frame - Red, 48 | 6 | 2011 | 16 |  | 0 |
| ML Road Frame - Red, 52 | 12 | 2011 | 37 | 81 | -54.3 |
| ML Road Frame - Red, 52 | 11 | 2011 | 81 | 102 | -20.6 |
| ML Road Frame - Red, 52 | 10 | 2011 | 102 | 43 | 137.2 |
| ML Road Frame - Red, 52 | 9 | 2011 | 43 | 51 | -15.7 |
| ML Road Frame - Red, 52 | 8 | 2011 | 51 | 78 | -34.6 |
| ML Road Frame - Red, 52 | 7 | 2011 | 78 | 50 | 56 |
| ML Road Frame - Red, 52 | 6 | 2011 | 50 |  | 0 |
| ML Road Frame - Red, 58 | 12 | 2011 | 29 | 55 | -47.3 |
| ML Road Frame - Red, 58 | 11 | 2011 | 55 | 81 | -32.1 |
| ML Road Frame - Red, 58 | 10 | 2011 | 81 | 26 | 211.5 |
| ML Road Frame - Red, 58 | 9 | 2011 | 26 | 37 | -29.7 |
| ML Road Frame - Red, 58 | 8 | 2011 | 37 | 54 | -31.5 |
| ML Road Frame - Red, 58 | 7 | 2011 | 54 | 27 | 100 |
| ML Road Frame - Red, 58 | 6 | 2011 | 27 |  | 0 |
| ML Road Frame - Red, 60 | 12 | 2011 | 18 | 32 | -43.8 |
| ML Road Frame - Red, 60 | 11 | 2011 | 32 | 47 | -31.9 |
| ML Road Frame - Red, 60 | 10 | 2011 | 47 | 22 | 113.6 |
| ML Road Frame - Red, 60 | 9 | 2011 | 22 | 27 | -18.5 |
| ML Road Frame - Red, 60 | 8 | 2011 | 27 | 22 | 22.7 |
| ML Road Frame - Red, 60 | 7 | 2011 | 22 | 11 | 100 |
| ML Road Frame - Red, 60 | 6 | 2011 | 11 |  | 0 |
| ML Road Front Wheel | 12 | 2011 | 113 | 202 | -44.1 |
| ML Road Front Wheel | 11 | 2011 | 202 | 291 | -30.6 |
| ML Road Front Wheel | 10 | 2011 | 291 | 110 | 164.5 |
| ML Road Front Wheel | 9 | 2011 | 110 | 128 | -14.1 |
| ML Road Front Wheel | 8 | 2011 | 128 | 169 | -24.3 |
| ML Road Front Wheel | 7 | 2011 | 169 | 98 | 72.4 |
| ML Road Front Wheel | 6 | 2011 | 98 |  | 0 |
| ML Road Handlebars | 12 | 2011 | 113 | 207 | -45.4 |
| ML Road Handlebars | 11 | 2011 | 207 | 291 | -28.9 |
| ML Road Handlebars | 10 | 2011 | 291 | 110 | 164.5 |
| ML Road Handlebars | 9 | 2011 | 110 | 125 | -12 |
| ML Road Handlebars | 8 | 2011 | 125 | 169 | -26 |
| ML Road Handlebars | 7 | 2011 | 169 | 98 | 72.4 |
| ML Road Handlebars | 6 | 2011 | 98 |  | 0 |
| ML Road Rear Wheel | 12 | 2011 | 113 | 207 | -45.4 |
| ML Road Rear Wheel | 11 | 2011 | 207 | 291 | -28.9 |
| ML Road Rear Wheel | 10 | 2011 | 291 | 110 | 164.5 |
| ML Road Rear Wheel | 9 | 2011 | 110 | 128 | -14.1 |
| ML Road Rear Wheel | 8 | 2011 | 128 | 169 | -24.3 |
| ML Road Rear Wheel | 7 | 2011 | 169 | 98 | 72.4 |
| ML Road Rear Wheel | 6 | 2011 | 98 |  | 0 |
| ML Road Seat Assembly | 12 | 2011 | 113 | 207 | -45.4 |
| ML Road Seat Assembly | 11 | 2011 | 207 | 291 | -28.9 |
| ML Road Seat Assembly | 10 | 2011 | 291 | 110 | 164.5 |
| ML Road Seat Assembly | 9 | 2011 | 110 | 128 | -14.1 |
| ML Road Seat Assembly | 8 | 2011 | 128 | 169 | -24.3 |
| ML Road Seat Assembly | 7 | 2011 | 169 | 97 | 74.2 |
| ML Road Seat Assembly | 6 | 2011 | 97 |  | 0 |
| Mountain End Caps | 12 | 2011 | 408 | 974 | -58.1 |
| Mountain End Caps | 11 | 2011 | 974 | 1314 | -25.9 |
| Mountain End Caps | 10 | 2011 | 1314 | 415 | 216.6 |
| Mountain End Caps | 9 | 2011 | 415 | 792 | -47.6 |
| Mountain End Caps | 8 | 2011 | 792 | 1040 | -23.8 |
| Mountain End Caps | 7 | 2011 | 1040 | 256 | 306.3 |
| Mountain End Caps | 6 | 2011 | 256 |  | 0 |
| Mountain-100 Black, 38 | 12 | 2011 | 28 | 66 | -57.6 |
| Mountain-100 Black, 38 | 11 | 2011 | 66 | 82 | -19.5 |
| Mountain-100 Black, 38 | 10 | 2011 | 82 | 34 | 141.2 |
| Mountain-100 Black, 38 | 9 | 2011 | 34 | 52 | -34.6 |
| Mountain-100 Black, 38 | 8 | 2011 | 52 | 71 | -26.8 |
| Mountain-100 Black, 38 | 7 | 2011 | 71 | 22 | 222.7 |
| Mountain-100 Black, 38 | 6 | 2011 | 22 |  | 0 |
| Mountain-100 Black, 42 | 12 | 2011 | 22 | 67 | -67.2 |
| Mountain-100 Black, 42 | 11 | 2011 | 67 | 76 | -11.8 |
| Mountain-100 Black, 42 | 10 | 2011 | 76 | 30 | 153.3 |
| Mountain-100 Black, 42 | 9 | 2011 | 30 | 44 | -31.8 |
| Mountain-100 Black, 42 | 8 | 2011 | 44 | 77 | -42.9 |
| Mountain-100 Black, 42 | 7 | 2011 | 77 | 16 | 381.3 |
| Mountain-100 Black, 42 | 6 | 2011 | 16 |  | 0 |
| Mountain-100 Black, 44 | 12 | 2011 | 28 | 69 | -59.4 |
| Mountain-100 Black, 44 | 11 | 2011 | 69 | 96 | -28.1 |
| Mountain-100 Black, 44 | 10 | 2011 | 96 | 26 | 269.2 |
| Mountain-100 Black, 44 | 9 | 2011 | 26 | 58 | -55.2 |
| Mountain-100 Black, 44 | 8 | 2011 | 58 | 68 | -14.7 |
| Mountain-100 Black, 44 | 7 | 2011 | 68 | 23 | 195.7 |
| Mountain-100 Black, 44 | 6 | 2011 | 23 |  | 0 |
| Mountain-100 Black, 48 | 12 | 2011 | 27 | 59 | -54.2 |
| Mountain-100 Black, 48 | 11 | 2011 | 59 | 79 | -25.3 |
| Mountain-100 Black, 48 | 10 | 2011 | 79 | 23 | 243.5 |
| Mountain-100 Black, 48 | 9 | 2011 | 23 | 46 | -50 |
| Mountain-100 Black, 48 | 8 | 2011 | 46 | 63 | -27 |
| Mountain-100 Black, 48 | 7 | 2011 | 63 | 21 | 200 |
| Mountain-100 Black, 48 | 6 | 2011 | 21 |  | 0 |
| Mountain-100 Silver, 38 | 12 | 2011 | 30 | 55 | -45.5 |
| Mountain-100 Silver, 38 | 11 | 2011 | 55 | 88 | -37.5 |
| Mountain-100 Silver, 38 | 10 | 2011 | 88 | 25 | 252 |
| Mountain-100 Silver, 38 | 9 | 2011 | 25 | 60 | -58.3 |
| Mountain-100 Silver, 38 | 8 | 2011 | 60 | 76 | -21.1 |
| Mountain-100 Silver, 38 | 7 | 2011 | 76 | 11 | 590.9 |
| Mountain-100 Silver, 38 | 6 | 2011 | 11 |  | 0 |
| Mountain-100 Silver, 42 | 12 | 2011 | 17 | 48 | -64.6 |
| Mountain-100 Silver, 42 | 11 | 2011 | 48 | 96 | -50 |
| Mountain-100 Silver, 42 | 10 | 2011 | 96 | 20 | 380 |
| Mountain-100 Silver, 42 | 9 | 2011 | 20 | 49 | -59.2 |
| Mountain-100 Silver, 42 | 8 | 2011 | 49 | 59 | -16.9 |
| Mountain-100 Silver, 42 | 7 | 2011 | 59 | 5 | 1,08 |
| Mountain-100 Silver, 42 | 6 | 2011 | 5 |  | 0 |
| Mountain-100 Silver, 44 | 12 | 2011 | 36 | 63 | -42.9 |
| Mountain-100 Silver, 44 | 11 | 2011 | 63 | 70 | -10 |
| Mountain-100 Silver, 44 | 10 | 2011 | 70 | 24 | 191.7 |
| Mountain-100 Silver, 44 | 9 | 2011 | 24 | 50 | -52 |
| Mountain-100 Silver, 44 | 8 | 2011 | 50 | 62 | -19.4 |
| Mountain-100 Silver, 44 | 7 | 2011 | 62 | 18 | 244.4 |
| Mountain-100 Silver, 44 | 6 | 2011 | 18 |  | 0 |
| Mountain-100 Silver, 48 | 12 | 2011 | 21 | 55 | -61.8 |
| Mountain-100 Silver, 48 | 11 | 2011 | 55 | 68 | -19.1 |
| Mountain-100 Silver, 48 | 10 | 2011 | 68 | 26 | 161.5 |
| Mountain-100 Silver, 48 | 9 | 2011 | 26 | 37 | -29.7 |
| Mountain-100 Silver, 48 | 8 | 2011 | 37 | 44 | -15.9 |
| Mountain-100 Silver, 48 | 7 | 2011 | 44 | 8 | 450 |
| Mountain-100 Silver, 48 | 6 | 2011 | 8 |  | 0 |
| Rear Derailleur | 12 | 2011 | 861 | 1440 | -40.2 |
| Rear Derailleur | 11 | 2011 | 1440 | 1918 | -24.9 |
| Rear Derailleur | 10 | 2011 | 1918 | 874 | 119.5 |
| Rear Derailleur | 9 | 2011 | 874 | 969 | -9.8 |
| Rear Derailleur | 8 | 2011 | 969 | 1278 | -24.2 |
| Rear Derailleur | 7 | 2011 | 1278 | 501 | 155.1 |
| Rear Derailleur | 6 | 2011 | 501 |  | 0 |
| Road End Caps | 12 | 2011 | 1282 | 1936 | -33.8 |
| Road End Caps | 11 | 2011 | 1936 | 2522 | -23.2 |
| Road End Caps | 10 | 2011 | 2522 | 1348 | 87.1 |
| Road End Caps | 9 | 2011 | 1348 | 1142 | 18 |
| Road End Caps | 8 | 2011 | 1142 | 1528 | -25.3 |
| Road End Caps | 7 | 2011 | 1528 | 792 | 92.9 |
| Road End Caps | 6 | 2011 | 792 |  | 0 |
| Road-150 Red, 44 | 12 | 2011 | 38 | 45 | -15.6 |
| Road-150 Red, 44 | 11 | 2011 | 45 | 40 | 12.5 |
| Road-150 Red, 44 | 10 | 2011 | 40 | 35 | 14.3 |
| Road-150 Red, 44 | 9 | 2011 | 35 | 20 | 75 |
| Road-150 Red, 44 | 8 | 2011 | 20 | 17 | 17.6 |
| Road-150 Red, 44 | 7 | 2011 | 17 | 10 | 70 |
| Road-150 Red, 44 | 6 | 2011 | 10 |  | 0 |
| Road-150 Red, 48 | 12 | 2011 | 47 | 41 | 14.6 |
| Road-150 Red, 48 | 11 | 2011 | 41 | 45 | -8.9 |
| Road-150 Red, 48 | 10 | 2011 | 45 | 35 | 28.6 |
| Road-150 Red, 48 | 9 | 2011 | 35 | 18 | 94.4 |
| Road-150 Red, 48 | 8 | 2011 | 18 | 34 | -47.1 |
| Road-150 Red, 48 | 7 | 2011 | 34 | 4 | 750 |
| Road-150 Red, 48 | 6 | 2011 | 4 |  | 0 |
| Road-150 Red, 52 | 12 | 2011 | 35 | 41 | -14.6 |
| Road-150 Red, 52 | 11 | 2011 | 41 | 53 | -22.6 |
| Road-150 Red, 52 | 10 | 2011 | 53 | 29 | 82.8 |
| Road-150 Red, 52 | 9 | 2011 | 29 | 24 | 20.8 |
| Road-150 Red, 52 | 8 | 2011 | 24 | 15 | 60 |
| Road-150 Red, 52 | 7 | 2011 | 15 | 4 | 275 |
| Road-150 Red, 52 | 6 | 2011 | 4 |  | 0 |
| Road-150 Red, 56 | 12 | 2011 | 46 | 55 | -16.4 |
| Road-150 Red, 56 | 11 | 2011 | 55 | 74 | -25.7 |
| Road-150 Red, 56 | 10 | 2011 | 74 | 44 | 68.2 |
| Road-150 Red, 56 | 9 | 2011 | 44 | 46 | -4.3 |
| Road-150 Red, 56 | 8 | 2011 | 46 | 55 | -16.4 |
| Road-150 Red, 56 | 7 | 2011 | 55 | 21 | 161.9 |
| Road-150 Red, 56 | 6 | 2011 | 21 |  | 0 |
| Road-150 Red, 62 | 12 | 2011 | 51 | 54 | -5.6 |
| Road-150 Red, 62 | 11 | 2011 | 54 | 67 | -19.4 |
| Road-150 Red, 62 | 10 | 2011 | 67 | 44 | 52.3 |
| Road-150 Red, 62 | 9 | 2011 | 44 | 44 | 0 |
| Road-150 Red, 62 | 8 | 2011 | 44 | 29 | 51.7 |
| Road-150 Red, 62 | 7 | 2011 | 29 | 13 | 123.1 |
| Road-150 Red, 62 | 6 | 2011 | 13 |  | 0 |
| Road-450 Red, 44 | 12 | 2011 | 23 | 36 | -36.1 |
| Road-450 Red, 44 | 11 | 2011 | 36 | 46 | -21.7 |
| Road-450 Red, 44 | 10 | 2011 | 46 | 19 | 142.1 |
| Road-450 Red, 44 | 9 | 2011 | 19 | 21 | -9.5 |
| Road-450 Red, 44 | 8 | 2011 | 21 | 21 | 0 |
| Road-450 Red, 44 | 7 | 2011 | 21 | 14 | 50 |
| Road-450 Red, 44 | 6 | 2011 | 14 |  | 0 |
| Road-450 Red, 48 | 12 | 2011 | 6 | 14 | -57.1 |
| Road-450 Red, 48 | 11 | 2011 | 14 | 29 | -51.7 |
| Road-450 Red, 48 | 10 | 2011 | 29 | 9 | 222.2 |
| Road-450 Red, 48 | 9 | 2011 | 9 |  | 0 |
| Road-450 Red, 52 | 12 | 2011 | 37 | 70 | -47.1 |
| Road-450 Red, 52 | 11 | 2011 | 70 | 87 | -19.5 |
| Road-450 Red, 52 | 10 | 2011 | 87 | 34 | 155.9 |
| Road-450 Red, 52 | 9 | 2011 | 34 | 43 | -20.9 |
| Road-450 Red, 52 | 8 | 2011 | 43 | 72 | -40.3 |
| Road-450 Red, 52 | 7 | 2011 | 72 | 46 | 56.5 |
| Road-450 Red, 52 | 6 | 2011 | 46 |  | 0 |
| Road-450 Red, 58 | 12 | 2011 | 29 | 55 | -47.3 |
| Road-450 Red, 58 | 11 | 2011 | 55 | 82 | -32.9 |
| Road-450 Red, 58 | 10 | 2011 | 82 | 26 | 215.4 |
| Road-450 Red, 58 | 9 | 2011 | 26 | 37 | -29.7 |
| Road-450 Red, 58 | 8 | 2011 | 37 | 54 | -31.5 |
| Road-450 Red, 58 | 7 | 2011 | 54 | 27 | 100 |
| Road-450 Red, 58 | 6 | 2011 | 27 |  | 0 |
| Road-450 Red, 60 | 12 | 2011 | 18 | 32 | -43.8 |
| Road-450 Red, 60 | 11 | 2011 | 32 | 47 | -31.9 |
| Road-450 Red, 60 | 10 | 2011 | 47 | 22 | 113.6 |
| Road-450 Red, 60 | 9 | 2011 | 22 | 27 | -18.5 |
| Road-450 Red, 60 | 8 | 2011 | 27 | 22 | 22.7 |
| Road-450 Red, 60 | 7 | 2011 | 22 | 11 | 100 |
| Road-450 Red, 60 | 6 | 2011 | 11 |  | 0 |
| Road-650 Black, 44 | 12 | 2011 | 21 | 27 | -22.2 |
| Road-650 Black, 44 | 11 | 2011 | 27 | 42 | -35.7 |
| Road-650 Black, 44 | 10 | 2011 | 42 | 27 | 55.6 |
| Road-650 Black, 44 | 9 | 2011 | 27 | 17 | 58.8 |
| Road-650 Black, 44 | 8 | 2011 | 17 | 19 | -10.5 |
| Road-650 Black, 44 | 7 | 2011 | 19 | 13 | 46.2 |
| Road-650 Black, 44 | 6 | 2011 | 13 |  | 0 |
| Road-650 Black, 48 | 12 | 2011 | 8 | 15 | -46.7 |
| Road-650 Black, 48 | 11 | 2011 | 15 | 30 | -50 |
| Road-650 Black, 48 | 10 | 2011 | 30 | 19 | 57.9 |
| Road-650 Black, 48 | 9 | 2011 | 19 |  | 0 |
| Road-650 Black, 52 | 12 | 2011 | 53 | 56 | -5.4 |
| Road-650 Black, 52 | 11 | 2011 | 56 | 89 | -37.1 |
| Road-650 Black, 52 | 10 | 2011 | 89 | 51 | 74.5 |
| Road-650 Black, 52 | 9 | 2011 | 51 | 48 | 6.3 |
| Road-650 Black, 52 | 8 | 2011 | 48 | 86 | -44.2 |
| Road-650 Black, 52 | 7 | 2011 | 86 | 29 | 196.6 |
| Road-650 Black, 52 | 6 | 2011 | 29 |  | 0 |
| Road-650 Black, 58 | 12 | 2011 | 43 | 49 | -12.2 |
| Road-650 Black, 58 | 11 | 2011 | 49 | 70 | -30 |
| Road-650 Black, 58 | 10 | 2011 | 70 | 43 | 62.8 |
| Road-650 Black, 58 | 9 | 2011 | 43 | 27 | 59.3 |
| Road-650 Black, 58 | 8 | 2011 | 27 | 49 | -44.9 |
| Road-650 Black, 58 | 7 | 2011 | 49 | 31 | 58.1 |
| Road-650 Black, 58 | 6 | 2011 | 31 |  | 0 |
| Road-650 Black, 60 | 12 | 2011 | 18 | 25 | -28 |
| Road-650 Black, 60 | 11 | 2011 | 25 | 45 | -44.4 |
| Road-650 Black, 60 | 10 | 2011 | 45 | 32 | 40.6 |
| Road-650 Black, 60 | 9 | 2011 | 32 | 18 | 77.8 |
| Road-650 Black, 60 | 8 | 2011 | 18 | 16 | 12.5 |
| Road-650 Black, 60 | 7 | 2011 | 16 | 11 | 45.5 |
| Road-650 Black, 60 | 6 | 2011 | 11 |  | 0 |
| Road-650 Black, 62 | 12 | 2011 | 9 | 21 | -57.1 |
| Road-650 Black, 62 | 11 | 2011 | 21 | 35 | -40 |
| Road-650 Black, 62 | 10 | 2011 | 35 | 16 | 118.8 |
| Road-650 Black, 62 | 9 | 2011 | 16 | 2 | 700 |
| Road-650 Black, 62 | 8 | 2011 | 2 | 1 | 100 |
| Road-650 Black, 62 | 7 | 2011 | 1 | 2 | -50 |
| Road-650 Black, 62 | 6 | 2011 | 2 |  | 0 |
| Road-650 Red, 44 | 12 | 2011 | 40 | 92 | -56.5 |
| Road-650 Red, 44 | 11 | 2011 | 92 | 64 | 43.8 |
| Road-650 Red, 44 | 10 | 2011 | 64 | 38 | 68.4 |
| Road-650 Red, 44 | 9 | 2011 | 38 | 49 | -22.4 |
| Road-650 Red, 44 | 8 | 2011 | 49 | 66 | -25.8 |
| Road-650 Red, 44 | 7 | 2011 | 66 | 44 | 50 |
| Road-650 Red, 44 | 6 | 2011 | 44 |  | 0 |
| Road-650 Red, 48 | 12 | 2011 | 31 | 54 | -42.6 |
| Road-650 Red, 48 | 11 | 2011 | 54 | 68 | -20.6 |
| Road-650 Red, 48 | 10 | 2011 | 68 | 41 | 65.9 |
| Road-650 Red, 48 | 9 | 2011 | 41 | 34 | 20.6 |
| Road-650 Red, 48 | 8 | 2011 | 34 | 54 | -37 |
| Road-650 Red, 48 | 7 | 2011 | 54 | 20 | 170 |
| Road-650 Red, 48 | 6 | 2011 | 20 |  | 0 |
| Road-650 Red, 52 | 12 | 2011 | 23 | 30 | -23.3 |
| Road-650 Red, 52 | 11 | 2011 | 30 | 50 | -40 |
| Road-650 Red, 52 | 10 | 2011 | 50 | 21 | 138.1 |
| Road-650 Red, 52 | 9 | 2011 | 21 | 14 | 50 |
| Road-650 Red, 52 | 8 | 2011 | 14 | 16 | -12.5 |
| Road-650 Red, 52 | 7 | 2011 | 16 | 15 | 6.7 |
| Road-650 Red, 52 | 6 | 2011 | 15 |  | 0 |
| Road-650 Red, 58 | 12 | 2011 | 12 | 19 | -36.8 |
| Road-650 Red, 58 | 11 | 2011 | 19 | 33 | -42.4 |
| Road-650 Red, 58 | 10 | 2011 | 33 | 7 | 371.4 |
| Road-650 Red, 58 | 9 | 2011 | 7 | 1 | 600 |
| Road-650 Red, 58 | 8 | 2011 | 1 |  | 0 |
| Road-650 Red, 60 | 12 | 2011 | 33 | 67 | -50.7 |
| Road-650 Red, 60 | 11 | 2011 | 67 | 88 | -23.9 |
| Road-650 Red, 60 | 10 | 2011 | 88 | 40 | 120 |
| Road-650 Red, 60 | 9 | 2011 | 40 | 47 | -14.9 |
| Road-650 Red, 60 | 8 | 2011 | 47 | 70 | -32.9 |
| Road-650 Red, 60 | 7 | 2011 | 70 | 43 | 62.8 |
| Road-650 Red, 60 | 6 | 2011 | 43 |  | 0 |
| Road-650 Red, 62 | 12 | 2011 | 31 | 56 | -44.6 |
| Road-650 Red, 62 | 11 | 2011 | 56 | 73 | -23.3 |
| Road-650 Red, 62 | 10 | 2011 | 73 | 34 | 114.7 |
| Road-650 Red, 62 | 9 | 2011 | 34 | 36 | -5.6 |
| Road-650 Red, 62 | 8 | 2011 | 36 | 62 | -41.9 |
| Road-650 Red, 62 | 7 | 2011 | 62 | 19 | 226.3 |
| Road-650 Red, 62 | 6 | 2011 | 19 |  | 0 |
| Seat Stays | 12 | 2011 | 3682 | 7195 | -48.8 |
| Seat Stays | 11 | 2011 | 7195 | 9340 | -23 |
| Seat Stays | 10 | 2011 | 9340 | 4243 | 120.1 |
| Seat Stays | 9 | 2011 | 4243 | 4764 | -10.9 |
| Seat Stays | 8 | 2011 | 4764 | 6332 | -24.8 |
| Seat Stays | 7 | 2011 | 6332 | 2560 | 147.3 |
| Seat Stays | 6 | 2011 | 2560 |  | 0 |
| Seat Tube | 12 | 2011 | 921 | 1799 | -48.8 |
| Seat Tube | 11 | 2011 | 1799 | 2335 | -23 |
| Seat Tube | 10 | 2011 | 2335 | 1061 | 120.1 |
| Seat Tube | 9 | 2011 | 1061 | 1191 | -10.9 |
| Seat Tube | 8 | 2011 | 1191 | 1583 | -24.8 |
| Seat Tube | 7 | 2011 | 1583 | 640 | 147.3 |
| Seat Tube | 6 | 2011 | 640 |  | 0 |
| Steerer | 12 | 2011 | 921 | 1799 | -48.8 |
| Steerer | 11 | 2011 | 1799 | 2270 | -20.7 |
| Steerer | 10 | 2011 | 2270 | 1061 | 113.9 |
| Steerer | 9 | 2011 | 1061 | 1191 | -10.9 |
| Steerer | 8 | 2011 | 1191 | 1583 | -24.8 |
| Steerer | 7 | 2011 | 1583 | 640 | 147.3 |
| Steerer | 6 | 2011 | 640 |  | 0 |
| Stem | 12 | 2011 | 848 | 1455 | -41.7 |
| Stem | 11 | 2011 | 1455 | 1918 | -24.1 |
| Stem | 10 | 2011 | 1918 | 885 | 116.7 |
| Stem | 9 | 2011 | 885 | 967 | -8.5 |
| Stem | 8 | 2011 | 967 | 1284 | -24.7 |
| Stem | 7 | 2011 | 1284 | 526 | 144.1 |
| Stem | 6 | 2011 | 526 |  | 0 |
| Top Tube | 12 | 2011 | 921 | 1767 | -47.9 |
| Top Tube | 11 | 2011 | 1767 | 2335 | -24.3 |
| Top Tube | 10 | 2011 | 2335 | 1061 | 120.1 |
| Top Tube | 9 | 2011 | 1061 | 1191 | -10.9 |
| Top Tube | 8 | 2011 | 1191 | 1583 | -24.8 |
| Top Tube | 7 | 2011 | 1583 | 640 | 147.3 |
| Top Tube | 6 | 2011 | 640 |  | 0 |

*Findings & Recommendations*
- Findings
  + Significant monthly stock fluctuations cause overstock and stockouts (e.g., BB Ball Bearing, Blade)
  + Overproduction leads to high inventory costs
  + Inconsistent inventory management and poor seasonal forecasting create inefficiencies
- Recommendations 
  + Forecast demand monthly using recent sales data to adjust inventory proactively
  + Maintain safety stock for high-variability products to prevent stockouts
  + Monitor monthly stock changes regularly and investigate unusual spikes or drops
  + Collaborate with suppliers for flexible, responsive replenishment
  + Plan production around seasonal demand patterns to balance stock levels


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
| mth | yr | ProductId | Name | sales | stock | ratio |
| --- | --- | --- | --- | --- | --- | --- |
| 12 | 2011 | 745 | HL Mountain Frame - Black, 48 | 1 | 27 | 27 |
| 12 | 2011 | 743 | HL Mountain Frame - Black, 42 | 1 | 26 | 26 |
| 12 | 2011 | 748 | HL Mountain Frame - Silver, 38 | 2 | 32 | 16 |
| 12 | 2011 | 722 | LL Road Frame - Black, 58 | 4 | 47 | 11.75 |
| 12 | 2011 | 747 | HL Mountain Frame - Black, 38 | 3 | 31 | 10.33 |
| 12 | 2011 | 726 | LL Road Frame - Red, 48 | 5 | 36 | 7.2 |
| 12 | 2011 | 738 | LL Road Frame - Black, 52 | 10 | 64 | 6.4 |
| 12 | 2011 | 730 | LL Road Frame - Red, 62 | 7 | 38 | 5.43 |
| 12 | 2011 | 741 | HL Mountain Frame - Silver, 48 | 5 | 27 | 5.4 |
| 12 | 2011 | 725 | LL Road Frame - Red, 44 | 12 | 53 | 4.42 |
| 12 | 2011 | 729 | LL Road Frame - Red, 60 | 10 | 43 | 4.3 |
| 12 | 2011 | 732 | ML Road Frame - Red, 48 | 10 | 16 | 1.6 |
| 12 | 2011 | 750 | Road-150 Red, 44 | 25 | 38 | 1.52 |
| 12 | 2011 | 751 | Road-150 Red, 48 | 32 | 47 | 1.47 |
| 12 | 2011 | 775 | Mountain-100 Black, 38 | 23 | 28 | 1.22 |
| 12 | 2011 | 749 | Road-150 Red, 62 | 45 | 51 | 1.13 |
| 12 | 2011 | 773 | Mountain-100 Silver, 44 | 32 | 36 | 1.13 |
| 12 | 2011 | 768 | Road-650 Black, 44 | 19 | 21 | 1.11 |
| 12 | 2011 | 765 | Road-650 Black, 58 | 39 | 43 | 1.1 |
| 12 | 2011 | 752 | Road-150 Red, 52 | 32 | 35 | 01.09 |
| 12 | 2011 | 778 | Mountain-100 Black, 48 | 25 | 27 | 01.08 |
| 12 | 2011 | 760 | Road-650 Red, 60 | 31 | 33 | 01.06 |
| 12 | 2011 | 770 | Road-650 Black, 52 | 52 | 53 | 01.02 |
| 12 | 2011 | 755 | Road-450 Red, 60 | 18 | 18 | 1 |
| 12 | 2011 | 756 | Road-450 Red, 44 | 23 | 23 | 1 |
| 12 | 2011 | 764 | Road-650 Red, 52 | 23 | 23 | 1 |
| 12 | 2011 | 742 | HL Mountain Frame - Silver, 46 | 3 | 3 | 1 |
| 12 | 2011 | 758 | Road-450 Red, 52 | 37 | 37 | 1 |
| 12 | 2011 | 757 | Road-450 Red, 48 | 6 | 6 | 1 |
| 12 | 2011 | 754 | Road-450 Red, 58 | 29 | 29 | 1 |
| 12 | 2011 | 759 | Road-650 Red, 58 | 12 | 12 | 1 |
| 12 | 2011 | 761 | Road-650 Red, 62 | 31 | 31 | 1 |
| 12 | 2011 | 762 | Road-650 Red, 44 | 41 | 40 | 0.98 |
| 12 | 2011 | 777 | Mountain-100 Black, 44 | 29 | 28 | 0.97 |
| 12 | 2011 | 774 | Mountain-100 Silver, 48 | 22 | 21 | 0.95 |
| 12 | 2011 | 753 | Road-150 Red, 56 | 49 | 46 | 0.94 |
| 12 | 2011 | 763 | Road-650 Red, 48 | 33 | 31 | 0.94 |
| 12 | 2011 | 772 | Mountain-100 Silver, 42 | 18 | 17 | 0.94 |
| 12 | 2011 | 776 | Mountain-100 Black, 42 | 24 | 22 | 0.92 |
| 12 | 2011 | 771 | Mountain-100 Silver, 38 | 33 | 30 | 0.91 |
| 12 | 2011 | 767 | Road-650 Black, 62 | 10 | 9 | 0.9 |
| 12 | 2011 | 769 | Road-650 Black, 48 | 9 | 8 | 0.89 |
| 12 | 2011 | 766 | Road-650 Black, 60 | 22 | 18 | 0.82 |
| 12 | 2011 | 708 | Sport-100 Helmet, Black | 10 |  | 0 |
| 12 | 2011 | 716 | Long-Sleeve Logo Jersey, XL | 6 |  | 0 |
| 12 | 2011 | 711 | Sport-100 Helmet, Blue | 7 |  | 0 |
| 12 | 2011 | 712 | AWC Logo Cap | 25 |  | 0 |
| 12 | 2011 | 707 | Sport-100 Helmet, Red | 12 |  | 0 |
| 12 | 2011 | 709 | Mountain Bike Socks, M | 45 |  | 0 |
| 12 | 2011 | 715 | Long-Sleeve Logo Jersey, L | 29 |  | 0 |
| 12 | 2011 | 714 | Long-Sleeve Logo Jersey, M | 9 |  | 0 |
| 11 | 2011 | 761 | Road-650 Red, 62 | 1 | 56 | 56 |
| 11 | 2011 | 764 | Road-650 Red, 52 | 1 | 30 | 30 |
| 11 | 2011 | 772 | Mountain-100 Silver, 42 | 2 | 48 | 24 |
| 11 | 2011 | 767 | Road-650 Black, 62 | 1 | 21 | 21 |
| 11 | 2011 | 763 | Road-650 Red, 48 | 3 | 54 | 18 |
| 11 | 2011 | 760 | Road-650 Red, 60 | 4 | 67 | 16.75 |
| 11 | 2011 | 769 | Road-650 Black, 48 | 1 | 15 | 15 |
| 11 | 2011 | 770 | Road-650 Black, 52 | 4 | 56 | 14 |
| 11 | 2011 | 771 | Mountain-100 Silver, 38 | 4 | 55 | 13.75 |
| 11 | 2011 | 776 | Mountain-100 Black, 42 | 5 | 67 | 13.4 |
| 11 | 2011 | 766 | Road-650 Black, 60 | 2 | 25 | 12.5 |
| 11 | 2011 | 765 | Road-650 Black, 58 | 4 | 49 | 12.25 |
| 11 | 2011 | 778 | Mountain-100 Black, 48 | 5 | 59 | 11.8 |
| 11 | 2011 | 773 | Mountain-100 Silver, 44 | 6 | 63 | 10.5 |
| 11 | 2011 | 759 | Road-650 Red, 58 | 2 | 19 | 9.5 |
| 11 | 2011 | 777 | Mountain-100 Black, 44 | 8 | 69 | 8.63 |
| 11 | 2011 | 775 | Mountain-100 Black, 38 | 8 | 66 | 8.25 |
| 11 | 2011 | 768 | Road-650 Black, 44 | 4 | 27 | 6.75 |
| 11 | 2011 | 753 | Road-150 Red, 56 | 31 | 55 | 1.77 |
| 11 | 2011 | 752 | Road-150 Red, 52 | 25 | 41 | 1.64 |
| 11 | 2011 | 749 | Road-150 Red, 62 | 35 | 54 | 1.54 |
| 11 | 2011 | 750 | Road-150 Red, 44 | 34 | 45 | 1.32 |
| 11 | 2011 | 751 | Road-150 Red, 48 | 40 | 41 | 01.02 |
| 10 | 2011 | 723 | LL Road Frame - Black, 60 | 1 | 46 | 46 |
| 10 | 2011 | 744 | HL Mountain Frame - Black, 44 | 6 | 100 | 16.67 |
| 10 | 2011 | 739 | HL Mountain Frame - Silver, 42 | 7 | 97 | 13.86 |
| 10 | 2011 | 727 | LL Road Frame - Red, 52 | 4 | 50 | 12.5 |
| 10 | 2011 | 717 | HL Road Frame - Red, 62 | 8 | 66 | 8.25 |
| 10 | 2011 | 718 | HL Road Frame - Red, 44 | 6 | 41 | 6.83 |
| 10 | 2011 | 736 | LL Road Frame - Black, 44 | 9 | 45 | 5 |
| 10 | 2011 | 733 | ML Road Frame - Red, 52 | 26 | 102 | 3.92 |
| 10 | 2011 | 745 | HL Mountain Frame - Black, 48 | 33 | 96 | 2.91 |
| 10 | 2011 | 747 | HL Mountain Frame - Black, 38 | 35 | 94 | 2.69 |
| 10 | 2011 | 748 | HL Mountain Frame - Silver, 38 | 46 | 112 | 2.43 |
| 10 | 2011 | 743 | HL Mountain Frame - Black, 42 | 45 | 96 | 2.13 |
| 10 | 2011 | 741 | HL Mountain Frame - Silver, 48 | 45 | 90 | 2 |
| 10 | 2011 | 738 | LL Road Frame - Black, 52 | 69 | 133 | 1.93 |
| 10 | 2011 | 722 | LL Road Frame - Black, 58 | 57 | 107 | 1.88 |
| 10 | 2011 | 729 | LL Road Frame - Red, 60 | 70 | 126 | 1.8 |
| 10 | 2011 | 726 | LL Road Frame - Red, 48 | 63 | 107 | 1.7 |
| 10 | 2011 | 730 | LL Road Frame - Red, 62 | 62 | 105 | 1.69 |
| 10 | 2011 | 725 | LL Road Frame - Red, 44 | 67 | 106 | 1.58 |
| 10 | 2011 | 732 | ML Road Frame - Red, 48 | 70 | 68 | 0.97 |
| 10 | 2011 | 751 | Road-150 Red, 48 | 57 | 45 | 0.79 |
| 10 | 2011 | 749 | Road-150 Red, 62 | 92 | 67 | 0.73 |
| 10 | 2011 | 753 | Road-150 Red, 56 | 104 | 74 | 0.71 |
| 10 | 2011 | 752 | Road-150 Red, 52 | 75 | 53 | 0.71 |
| 10 | 2011 | 766 | Road-650 Black, 60 | 66 | 45 | 0.68 |
| 10 | 2011 | 772 | Mountain-100 Silver, 42 | 141 | 96 | 0.68 |
| 10 | 2011 | 759 | Road-650 Red, 58 | 49 | 33 | 0.67 |
| 10 | 2011 | 757 | Road-450 Red, 48 | 43 | 29 | 0.67 |
| 10 | 2011 | 769 | Road-650 Black, 48 | 45 | 30 | 0.67 |
| 10 | 2011 | 767 | Road-650 Black, 62 | 55 | 35 | 0.64 |
| 10 | 2011 | 764 | Road-650 Red, 52 | 78 | 50 | 0.64 |
| 10 | 2011 | 750 | Road-150 Red, 44 | 65 | 40 | 0.62 |
| 10 | 2011 | 768 | Road-650 Black, 44 | 68 | 42 | 0.62 |
| 10 | 2011 | 771 | Mountain-100 Silver, 38 | 141 | 88 | 0.62 |
| 10 | 2011 | 770 | Road-650 Black, 52 | 145 | 89 | 0.61 |
| 10 | 2011 | 777 | Mountain-100 Black, 44 | 158 | 96 | 0.61 |
| 10 | 2011 | 754 | Road-450 Red, 58 | 137 | 82 | 0.6 |
| 10 | 2011 | 765 | Road-650 Black, 58 | 117 | 70 | 0.6 |
| 10 | 2011 | 755 | Road-450 Red, 60 | 79 | 47 | 0.59 |
| 10 | 2011 | 778 | Mountain-100 Black, 48 | 134 | 79 | 0.59 |
| 10 | 2011 | 763 | Road-650 Red, 48 | 120 | 68 | 0.57 |
| 10 | 2011 | 775 | Mountain-100 Black, 38 | 145 | 82 | 0.57 |
| 10 | 2011 | 761 | Road-650 Red, 62 | 129 | 73 | 0.57 |
| 10 | 2011 | 760 | Road-650 Red, 60 | 154 | 88 | 0.57 |
| 10 | 2011 | 756 | Road-450 Red, 44 | 82 | 46 | 0.56 |
| 10 | 2011 | 774 | Mountain-100 Silver, 48 | 123 | 68 | 0.55 |
| 10 | 2011 | 758 | Road-450 Red, 52 | 157 | 87 | 0.55 |
| 10 | 2011 | 776 | Mountain-100 Black, 42 | 140 | 76 | 0.54 |
| 10 | 2011 | 773 | Mountain-100 Silver, 44 | 132 | 70 | 0.53 |
| 10 | 2011 | 742 | HL Mountain Frame - Silver, 46 | 32 | 17 | 0.53 |
| 10 | 2011 | 762 | Road-650 Red, 44 | 156 | 64 | 0.41 |
| 10 | 2011 | 711 | Sport-100 Helmet, Blue | 181 |  | 0 |
| 10 | 2011 | 707 | Sport-100 Helmet, Red | 141 |  | 0 |
| 10 | 2011 | 716 | Long-Sleeve Logo Jersey, XL | 117 |  | 0 |
| 10 | 2011 | 709 | Mountain Bike Socks, M | 224 |  | 0 |
| 10 | 2011 | 714 | Long-Sleeve Logo Jersey, M | 101 |  | 0 |
| 10 | 2011 | 712 | AWC Logo Cap | 240 |  | 0 |
| 10 | 2011 | 715 | Long-Sleeve Logo Jersey, L | 239 |  | 0 |
| 10 | 2011 | 708 | Sport-100 Helmet, Black | 162 |  | 0 |
| 10 | 2011 | 710 | Mountain Bike Socks, L | 29 |  | 0 |
| 9 | 2011 | 763 | Road-650 Red, 48 | 1 | 41 | 41 |
| 9 | 2011 | 761 | Road-650 Red, 62 | 1 | 34 | 34 |
| 9 | 2011 | 771 | Mountain-100 Silver, 38 | 1 | 25 | 25 |
| 9 | 2011 | 773 | Mountain-100 Silver, 44 | 1 | 24 | 24 |
| 9 | 2011 | 765 | Road-650 Black, 58 | 2 | 43 | 21.5 |
| 9 | 2011 | 760 | Road-650 Red, 60 | 2 | 40 | 20 |
| 9 | 2011 | 762 | Road-650 Red, 44 | 2 | 38 | 19 |
| 9 | 2011 | 775 | Mountain-100 Black, 38 | 2 | 34 | 17 |
| 9 | 2011 | 774 | Mountain-100 Silver, 48 | 2 | 26 | 13 |
| 9 | 2011 | 766 | Road-650 Black, 60 | 3 | 32 | 10.67 |
| 9 | 2011 | 764 | Road-650 Red, 52 | 2 | 21 | 10.5 |
| 9 | 2011 | 776 | Mountain-100 Black, 42 | 3 | 30 | 10 |
| 9 | 2011 | 777 | Mountain-100 Black, 44 | 3 | 26 | 8.67 |
| 9 | 2011 | 767 | Road-650 Black, 62 | 2 | 16 | 8 |
| 9 | 2011 | 778 | Mountain-100 Black, 48 | 7 | 23 | 3.29 |
| 9 | 2011 | 772 | Mountain-100 Silver, 42 | 7 | 20 | 2.86 |
| 9 | 2011 | 753 | Road-150 Red, 56 | 23 | 44 | 1.91 |
| 9 | 2011 | 759 | Road-650 Red, 58 | 4 | 7 | 1.75 |
| 9 | 2011 | 750 | Road-150 Red, 44 | 21 | 35 | 1.67 |
| 9 | 2011 | 749 | Road-150 Red, 62 | 27 | 44 | 1.63 |
| 9 | 2011 | 752 | Road-150 Red, 52 | 18 | 29 | 1.61 |
| 9 | 2011 | 751 | Road-150 Red, 48 | 23 | 35 | 1.52 |
| 8 | 2011 | 744 | HL Mountain Frame - Black, 44 | 3 | 60 | 20 |
| 8 | 2011 | 727 | LL Road Frame - Red, 52 | 1 | 14 | 14 |
| 8 | 2011 | 717 | HL Road Frame - Red, 62 | 5 | 42 | 8.4 |
| 8 | 2011 | 739 | HL Mountain Frame - Silver, 42 | 6 | 49 | 8.17 |
| 8 | 2011 | 736 | LL Road Frame - Black, 44 | 4 | 18 | 4.5 |
| 8 | 2011 | 747 | HL Mountain Frame - Black, 38 | 17 | 69 | 04.06 |
| 8 | 2011 | 718 | HL Road Frame - Red, 44 | 5 | 19 | 3.8 |
| 8 | 2011 | 748 | HL Mountain Frame - Silver, 38 | 21 | 73 | 3.48 |
| 8 | 2011 | 745 | HL Mountain Frame - Black, 48 | 18 | 61 | 3.39 |
| 8 | 2011 | 733 | ML Road Frame - Red, 52 | 17 | 51 | 3 |
| 8 | 2011 | 741 | HL Mountain Frame - Silver, 48 | 24 | 57 | 2.38 |
| 8 | 2011 | 743 | HL Mountain Frame - Black, 42 | 28 | 64 | 2.29 |
| 8 | 2011 | 726 | LL Road Frame - Red, 48 | 18 | 41 | 2.28 |
| 8 | 2011 | 730 | LL Road Frame - Red, 62 | 23 | 44 | 1.91 |
| 8 | 2011 | 729 | LL Road Frame - Red, 60 | 44 | 72 | 1.64 |
| 8 | 2011 | 738 | LL Road Frame - Black, 52 | 44 | 71 | 1.61 |
| 8 | 2011 | 722 | LL Road Frame - Black, 58 | 23 | 34 | 1.48 |
| 8 | 2011 | 725 | LL Road Frame - Red, 44 | 53 | 78 | 1.47 |
| 8 | 2011 | 742 | HL Mountain Frame - Silver, 46 | 21 | 18 | 0.86 |
| 8 | 2011 | 749 | Road-150 Red, 62 | 55 | 44 | 0.8 |
| 8 | 2011 | 771 | Mountain-100 Silver, 38 | 77 | 60 | 0.78 |
| 8 | 2011 | 778 | Mountain-100 Black, 48 | 61 | 46 | 0.75 |
| 8 | 2011 | 772 | Mountain-100 Silver, 42 | 66 | 49 | 0.74 |
| 8 | 2011 | 752 | Road-150 Red, 52 | 33 | 24 | 0.73 |
| 8 | 2011 | 777 | Mountain-100 Black, 44 | 80 | 58 | 0.72 |
| 8 | 2011 | 753 | Road-150 Red, 56 | 66 | 46 | 0.7 |
| 8 | 2011 | 773 | Mountain-100 Silver, 44 | 74 | 50 | 0.68 |
| 8 | 2011 | 776 | Mountain-100 Black, 42 | 69 | 44 | 0.64 |
| 8 | 2011 | 775 | Mountain-100 Black, 38 | 82 | 52 | 0.63 |
| 8 | 2011 | 774 | Mountain-100 Silver, 48 | 61 | 37 | 0.61 |
| 8 | 2011 | 754 | Road-450 Red, 58 | 63 | 37 | 0.59 |
| 8 | 2011 | 750 | Road-150 Red, 44 | 34 | 20 | 0.59 |
| 8 | 2011 | 762 | Road-650 Red, 44 | 86 | 49 | 0.57 |
| 8 | 2011 | 758 | Road-450 Red, 52 | 77 | 43 | 0.56 |
| 8 | 2011 | 732 | ML Road Frame - Red, 48 | 25 | 14 | 0.56 |
| 8 | 2011 | 755 | Road-450 Red, 60 | 49 | 27 | 0.55 |
| 8 | 2011 | 760 | Road-650 Red, 60 | 86 | 47 | 0.55 |
| 8 | 2011 | 756 | Road-450 Red, 44 | 40 | 21 | 0.53 |
| 8 | 2011 | 761 | Road-650 Red, 62 | 69 | 36 | 0.52 |
| 8 | 2011 | 770 | Road-650 Black, 52 | 95 | 48 | 0.51 |
| 8 | 2011 | 751 | Road-150 Red, 48 | 39 | 18 | 0.46 |
| 8 | 2011 | 763 | Road-650 Red, 48 | 74 | 34 | 0.46 |
| 8 | 2011 | 764 | Road-650 Red, 52 | 35 | 14 | 0.4 |
| 8 | 2011 | 768 | Road-650 Black, 44 | 42 | 17 | 0.4 |
| 8 | 2011 | 765 | Road-650 Black, 58 | 70 | 27 | 0.39 |
| 8 | 2011 | 766 | Road-650 Black, 60 | 49 | 18 | 0.37 |
| 8 | 2011 | 759 | Road-650 Red, 58 | 5 | 1 | 0.2 |
| 8 | 2011 | 767 | Road-650 Black, 62 | 16 | 2 | 0.13 |
| 8 | 2011 | 709 | Mountain Bike Socks, M | 167 |  | 0 |
| 8 | 2011 | 716 | Long-Sleeve Logo Jersey, XL | 65 |  | 0 |
| 8 | 2011 | 708 | Sport-100 Helmet, Black | 86 |  | 0 |
| 8 | 2011 | 757 | Road-450 Red, 48 | 9 |  | 0 |
| 8 | 2011 | 714 | Long-Sleeve Logo Jersey, M | 65 |  | 0 |
| 8 | 2011 | 769 | Road-650 Black, 48 | 19 |  | 0 |
| 8 | 2011 | 711 | Sport-100 Helmet, Blue | 75 |  | 0 |
| 8 | 2011 | 710 | Mountain Bike Socks, L | 19 |  | 0 |
| 8 | 2011 | 715 | Long-Sleeve Logo Jersey, L | 113 |  | 0 |
| 8 | 2011 | 712 | AWC Logo Cap | 137 |  | 0 |
| 8 | 2011 | 707 | Sport-100 Helmet, Red | 96 |  | 0 |
| 7 | 2011 | 733 | ML Road Frame - Red, 52 | 8 | 78 | 9.75 |
| 7 | 2011 | 743 | HL Mountain Frame - Black, 42 | 13 | 91 | 7 |
| 7 | 2011 | 747 | HL Mountain Frame - Black, 38 | 14 | 85 | 06.07 |
| 7 | 2011 | 730 | LL Road Frame - Red, 62 | 13 | 75 | 5.77 |
| 7 | 2011 | 748 | HL Mountain Frame - Silver, 38 | 20 | 96 | 4.8 |
| 7 | 2011 | 745 | HL Mountain Frame - Black, 48 | 19 | 83 | 4.37 |
| 7 | 2011 | 741 | HL Mountain Frame - Silver, 48 | 14 | 58 | 4.14 |
| 7 | 2011 | 726 | LL Road Frame - Red, 48 | 19 | 73 | 3.84 |
| 7 | 2011 | 722 | LL Road Frame - Black, 58 | 20 | 67 | 3.35 |
| 7 | 2011 | 738 | LL Road Frame - Black, 52 | 39 | 126 | 3.23 |
| 7 | 2011 | 725 | LL Road Frame - Red, 44 | 38 | 104 | 2.74 |
| 7 | 2011 | 729 | LL Road Frame - Red, 60 | 41 | 112 | 2.73 |
| 7 | 2011 | 751 | Road-150 Red, 48 | 18 | 34 | 1.89 |
| 7 | 2011 | 750 | Road-150 Red, 44 | 15 | 17 | 1.13 |
| 7 | 2011 | 773 | Mountain-100 Silver, 44 | 57 | 62 | 01.09 |
| 7 | 2011 | 764 | Road-650 Red, 52 | 15 | 16 | 01.07 |
| 7 | 2011 | 774 | Mountain-100 Silver, 48 | 42 | 44 | 01.05 |
| 7 | 2011 | 772 | Mountain-100 Silver, 42 | 57 | 59 | 01.04 |
| 7 | 2011 | 765 | Road-650 Black, 58 | 47 | 49 | 01.04 |
| 7 | 2011 | 777 | Mountain-100 Black, 44 | 66 | 68 | 01.03 |
| 7 | 2011 | 762 | Road-650 Red, 44 | 64 | 66 | 01.03 |
| 7 | 2011 | 761 | Road-650 Red, 62 | 61 | 62 | 01.02 |
| 7 | 2011 | 732 | ML Road Frame - Red, 48 | 26 | 26 | 1 |
| 7 | 2011 | 742 | HL Mountain Frame - Silver, 46 | 15 | 15 | 1 |
| 7 | 2011 | 763 | Road-650 Red, 48 | 54 | 54 | 1 |
| 7 | 2011 | 768 | Road-650 Black, 44 | 19 | 19 | 1 |
| 7 | 2011 | 775 | Mountain-100 Black, 38 | 71 | 71 | 1 |
| 7 | 2011 | 754 | Road-450 Red, 58 | 54 | 54 | 1 |
| 7 | 2011 | 758 | Road-450 Red, 52 | 72 | 72 | 1 |
| 7 | 2011 | 756 | Road-450 Red, 44 | 21 | 21 | 1 |
| 7 | 2011 | 755 | Road-450 Red, 60 | 22 | 22 | 1 |
| 7 | 2011 | 760 | Road-650 Red, 60 | 71 | 70 | 0.99 |
| 7 | 2011 | 778 | Mountain-100 Black, 48 | 64 | 63 | 0.98 |
| 7 | 2011 | 770 | Road-650 Black, 52 | 88 | 86 | 0.98 |
| 7 | 2011 | 776 | Mountain-100 Black, 42 | 81 | 77 | 0.95 |
| 7 | 2011 | 771 | Mountain-100 Silver, 38 | 81 | 76 | 0.94 |
| 7 | 2011 | 766 | Road-650 Black, 60 | 17 | 16 | 0.94 |
| 7 | 2011 | 753 | Road-150 Red, 56 | 61 | 55 | 0.9 |
| 7 | 2011 | 749 | Road-150 Red, 62 | 41 | 29 | 0.71 |
| 7 | 2011 | 752 | Road-150 Red, 52 | 21 | 15 | 0.71 |
| 7 | 2011 | 767 | Road-650 Black, 62 | 2 | 1 | 0.5 |
| 7 | 2011 | 712 | AWC Logo Cap | 103 |  | 0 |
| 7 | 2011 | 759 | Road-650 Red, 58 | 1 |  | 0 |
| 7 | 2011 | 710 | Mountain Bike Socks, L | 13 |  | 0 |
| 7 | 2011 | 711 | Sport-100 Helmet, Blue | 64 |  | 0 |
| 7 | 2011 | 715 | Long-Sleeve Logo Jersey, L | 114 |  | 0 |
| 7 | 2011 | 709 | Mountain Bike Socks, M | 134 |  | 0 |
| 7 | 2011 | 716 | Long-Sleeve Logo Jersey, XL | 48 |  | 0 |
| 7 | 2011 | 708 | Sport-100 Helmet, Black | 56 |  | 0 |
| 7 | 2011 | 714 | Long-Sleeve Logo Jersey, M | 37 |  | 0 |
| 7 | 2011 | 707 | Sport-100 Helmet, Red | 58 |  | 0 |
| 6 | 2011 | 762 | Road-650 Red, 44 | 2 | 44 | 22 |
| 6 | 2011 | 763 | Road-650 Red, 48 | 1 | 20 | 20 |
| 6 | 2011 | 761 | Road-650 Red, 62 | 1 | 19 | 19 |
| 6 | 2011 | 776 | Mountain-100 Black, 42 | 1 | 16 | 16 |
| 6 | 2011 | 770 | Road-650 Black, 52 | 2 | 29 | 14.5 |
| 6 | 2011 | 765 | Road-650 Black, 58 | 3 | 31 | 10.33 |
| 6 | 2011 | 764 | Road-650 Red, 52 | 2 | 15 | 7.5 |
| 6 | 2011 | 775 | Mountain-100 Black, 38 | 3 | 22 | 7.33 |
| 6 | 2011 | 778 | Mountain-100 Black, 48 | 3 | 21 | 7 |
| 6 | 2011 | 768 | Road-650 Black, 44 | 2 | 13 | 6.5 |
| 6 | 2011 | 777 | Mountain-100 Black, 44 | 6 | 23 | 3.83 |
| 6 | 2011 | 773 | Mountain-100 Silver, 44 | 6 | 18 | 3 |
| 6 | 2011 | 771 | Mountain-100 Silver, 38 | 4 | 11 | 2.75 |
| 6 | 2011 | 774 | Mountain-100 Silver, 48 | 3 | 8 | 2.67 |
| 6 | 2011 | 772 | Mountain-100 Silver, 42 | 2 | 5 | 2.5 |
| 6 | 2011 | 767 | Road-650 Black, 62 | 1 | 2 | 2 |
| 6 | 2011 | 753 | Road-150 Red, 56 | 15 | 21 | 1.4 |
| 6 | 2011 | 749 | Road-150 Red, 62 | 21 | 13 | 0.62 |
| 6 | 2011 | 750 | Road-150 Red, 44 | 23 | 10 | 0.43 |
| 6 | 2011 | 752 | Road-150 Red, 52 | 12 | 4 | 0.33 |
| 6 | 2011 | 751 | Road-150 Red, 48 | 28 | 4 | 0.14 |
| 5 | 2011 | 738 | LL Road Frame - Black, 52 | 19 |  | 0 |
| 5 | 2011 | 775 | Mountain-100 Black, 38 | 22 |  | 0 |
| 5 | 2011 | 747 | HL Mountain Frame - Black, 38 | 4 |  | 0 |
| 5 | 2011 | 756 | Road-450 Red, 44 | 14 |  | 0 |
| 5 | 2011 | 745 | HL Mountain Frame - Black, 48 | 1 |  | 0 |
| 5 | 2011 | 770 | Road-650 Black, 52 | 29 |  | 0 |
| 5 | 2011 | 732 | ML Road Frame - Red, 48 | 16 |  | 0 |
| 5 | 2011 | 765 | Road-650 Black, 58 | 30 |  | 0 |
| 5 | 2011 | 776 | Mountain-100 Black, 42 | 16 |  | 0 |
| 5 | 2011 | 707 | Sport-100 Helmet, Red | 24 |  | 0 |
| 5 | 2011 | 777 | Mountain-100 Black, 44 | 23 |  | 0 |
| 5 | 2011 | 773 | Mountain-100 Silver, 44 | 17 |  | 0 |
| 5 | 2011 | 749 | Road-150 Red, 62 | 4 |  | 0 |
| 5 | 2011 | 758 | Road-450 Red, 52 | 46 |  | 0 |
| 5 | 2011 | 771 | Mountain-100 Silver, 38 | 10 |  | 0 |
| 5 | 2011 | 755 | Road-450 Red, 60 | 11 |  | 0 |
| 5 | 2011 | 711 | Sport-100 Helmet, Blue | 33 |  | 0 |
| 5 | 2011 | 712 | AWC Logo Cap | 40 |  | 0 |
| 5 | 2011 | 722 | LL Road Frame - Black, 58 | 8 |  | 0 |
| 5 | 2011 | 730 | LL Road Frame - Red, 62 | 14 |  | 0 |
| 5 | 2011 | 726 | LL Road Frame - Red, 48 | 9 |  | 0 |
| 5 | 2011 | 762 | Road-650 Red, 44 | 44 |  | 0 |
| 5 | 2011 | 743 | HL Mountain Frame - Black, 42 | 1 |  | 0 |
| 5 | 2011 | 767 | Road-650 Black, 62 | 1 |  | 0 |
| 5 | 2011 | 754 | Road-450 Red, 58 | 27 |  | 0 |
| 5 | 2011 | 778 | Mountain-100 Black, 48 | 20 |  | 0 |
| 5 | 2011 | 709 | Mountain Bike Socks, M | 38 |  | 0 |
| 5 | 2011 | 753 | Road-150 Red, 56 | 14 |  | 0 |
| 5 | 2011 | 761 | Road-650 Red, 62 | 19 |  | 0 |
| 5 | 2011 | 766 | Road-650 Black, 60 | 11 |  | 0 |
| 5 | 2011 | 764 | Road-650 Red, 52 | 14 |  | 0 |
| 5 | 2011 | 710 | Mountain Bike Socks, L | 5 |  | 0 |
| 5 | 2011 | 715 | Long-Sleeve Logo Jersey, L | 49 |  | 0 |
| 5 | 2011 | 716 | Long-Sleeve Logo Jersey, XL | 19 |  | 0 |
| 5 | 2011 | 760 | Road-650 Red, 60 | 43 |  | 0 |
| 5 | 2011 | 748 | HL Mountain Frame - Silver, 38 | 2 |  | 0 |
| 5 | 2011 | 714 | Long-Sleeve Logo Jersey, M | 16 |  | 0 |
| 5 | 2011 | 742 | HL Mountain Frame - Silver, 46 | 3 |  | 0 |
| 5 | 2011 | 768 | Road-650 Black, 44 | 13 |  | 0 |
| 5 | 2011 | 708 | Sport-100 Helmet, Black | 27 |  | 0 |
| 5 | 2011 | 725 | LL Road Frame - Red, 44 | 15 |  | 0 |
| 5 | 2011 | 741 | HL Mountain Frame - Silver, 48 | 2 |  | 0 |
| 5 | 2011 | 763 | Road-650 Red, 48 | 20 |  | 0 |
| 5 | 2011 | 729 | LL Road Frame - Red, 60 | 16 |  | 0 |
| 5 | 2011 | 774 | Mountain-100 Silver, 48 | 7 |  | 0 |
| 5 | 2011 | 733 | ML Road Frame - Red, 52 | 4 |  | 0 |
| 5 | 2011 | 772 | Mountain-100 Silver, 42 | 5 |  | 0 |

*Findings & Recommendations*
- Findings 
  + High stock/sales ratios (>5) for many SKUs = slow-moving inventory, cash flow risk
  + Missing stock data for accessories (helmets, socks, jerseys) = inventory blind spots
  + Stock/sales ratio drops late in year = effective clearance or seasonal sales spike
  + Best-selling SKUs maintain balanced stock-to-sales (~1) = efficient inventory
- Recommendations
  + Reduce or clear high-ratio SKUs
  + Fix stock tracking for accessories
  + Prioritize inventory for fast-moving SKUs
  + Monitor ratios monthly for early issue detection


**QUERY 8 - Number of orders and value at Pending status in 2014**

*Purpose*: Monitor pending orders to uncover process bottlenecks, enhance fulfillment efficiency, and minimize revenue delays or customer dissatisfaction

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
*Result*
| yr | Status | order_cnt | value |
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










