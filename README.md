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
    → 🔎 This cyclical volatility hints at reactive inventory planning, possibly driven by inconsistent forecasting or sudden demand shifts.
  + Potential Overstock Risk on Seat Stays (Oct)
    ++ Seat Stays shows a sudden +120% surge in stock in October without further context.
    → 🔎 If not aligned with demand, this could indicate overstocking, which ties up working capital and increases carrying costs.
  + Drop after Spike Pattern = Inefficient Replenishment
    ++ The repeated pattern of stock surge → sharp drop (seen in BB Ball Bearing) might indicate:
    Overstocking due to panic replenishment
    Then understocking as consumption catches up
    → 🔎 This up-down cycle can create a bullwhip effect across the supply chain.
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










