Pizza Sales Analytics

A compact, reproducible project to analyze pizza sales and recreate the dashboard shown above using two approaches:
pure SQL, and 2) Excel (Power Query + Pivot/Charts).

1) Project Overview
Goal: Compute core KPIs, trends, and best/worst sellers; then visualize them.
Data: pizza_sales.csv (order-level data with date, time, category, size, quantity, price, etc.).
Outputs:
SQL result sets for KPIs & analyses
An Excel dashboard replicating: KPIs, daily/hourly trends, category/size mix, and top/bottom sellers.
The SQL approach follows the query set in PIZZA SALES SQL QUERIES.docx (KPI, trends, category/size mix, top/bottom sellers). 
2) Repository Structure (suggested)
.
├─ data/
│  └─ pizza_sales.csv
├─ sql/
│  └─ pizza_sales_queries.sql   # same logic as the DOCX, saved as .sql
├─ excel/
│  └─ Pizza_Sales_Dashboard.xlsx
├─ images/
│  └─ dashboard_preview.png
└─ README.md

3) Metrics & Analyses
KPIs:
# Total Revenue
# Average Order Value (AOV)
# Total Pizzas Sold
# Total Orders
# Average Pizzas per Order
# Analyses
# Daily trend of orders (Sun–Sat)
# Hourly trend of orders (0–23h)
# % of sales by Pizza Category
# % of sales by Pizza Size
# Total pizzas sold by category
# Top 5 best sellers (by pizzas sold)
# Bottom 5 sellers (by pizzas sold)
# Each of these has a corresponding SQL query in the provided query set. 

4) Prerequisites
For SQL route
Any SQL RDBMS that supports standard date functions (e.g., SQL Server / PostgreSQL / MySQL with minor function tweaks).
A database/schema you can write to.

For Excel route
Microsoft Excel (ideally Excel 365 or Excel 2019+).
Power Query (Get & Transform) enabled.

5) Getting Started — SQL Solution
Create table & load data
Create a table named pizza_sales with columns to match pizza_sales.csv
(e.g., order_id, order_date, order_time, pizza_name, pizza_category, pizza_size, quantity, total_price, …).
Bulk import data/pizza_sales.csv.
Run KPI Queries
# Total Revenue
SELECT SUM(total_price) AS Total_Revenue
FROM pizza_sales;
# Average Order Value
SELECT SUM(total_price) / COUNT(DISTINCT order_id) AS Avg_Order_Value
FROM pizza_sales;
# Total Pizzas Sold
SELECT SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales;
# Total Orders
SELECT COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales;
# Avg Pizzas per Order
SELECT CAST(SUM(quantity) AS DECIMAL(10,2)) /
       CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS Avg_Pizzas_Per_Order
FROM pizza_sales;
KPI query set reference. 
Run Trend & Mix Queries
Daily Trend (orders by weekday)
Use your SQL dialect’s weekday function (DATENAME(DW, order_date) / TO_CHAR(...,'Day') / etc.).
Hourly Trend (orders by hour)
Group by the hour extracted from order_time.
# % Sales by Category/Size
Compute revenue share = SUM(total_price) / SUM(total_price over all) × 100.
# Category Volume
SUM(quantity) grouped by pizza_category.
See the prepared statements for exact syntax and optional filters. 
#Top/Bottom Sellers
Top 5 by pizzas sold
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;

Bottom 5 by pizzas sold
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;
Top/Bottom seller queries mirror the provided doc. Adjust TOP / LIMIT for your SQL engine. 

Optional Filters
Add WHERE conditions for Month, Quarter, Week, or Date range.
Examples in the query set: WHERE MONTH(order_date) = 1 or WHERE DATEPART(QUARTER, order_date) = 1. 

Visualize (optional)
Export results to a BI tool (Power BI/Tableau) or Excel for charting, or build views to feed a dashboard.

6) Getting Started — Excel Solution
Import data
Open excel/Pizza_Sales_Dashboard.xlsx (or create a new workbook).
Data → Get Data → From Text/CSV → select data/pizza_sales.csv.
Use Power Query to set correct data types:
order_date → Date
order_time → Time
quantity → Whole Number
total_price → Decimal Number
Create Calculated Columns (in PQ or in the table)
# Order Day: weekday name from order_date
# Order Hour: hour extracted from order_time
(Optional) Month, Quarter, Year for slicers
Build Pivot Tables
KPI Helper Pivots
# Total Revenue: Sum of total_price
# Total Pizzas Sold: Sum of quantity
# Total Orders: Distinct Count of order_id (enable Data Model)
# AOV = Total Revenue / Total Orders (use a Pivot calc or a cube formula)
# Avg Pizzas/Order = Total Pizzas Sold / Total Orders
# Daily Trend: Rows = Order Day; Values = Distinct Count of order_id
# Hourly Trend: Rows = Order Hour; Values = Distinct Count of order_id
# % Sales by Category/Size:
# Values = Sum of total_price; Show Values As → “% of Grand Total”
# Category Volume: Rows = pizza_category; Values = Sum of quantity
#Top/Bottom Sellers:
Rows = pizza_name; Values = Sum of quantity; sort DESC/ASC and keep Top 5/Bottom 5 (Value Filters)
Create the Dashboard
Insert Cards/Shapes for KPIs (linked to pivot cells).
# Charts:
Column chart for Daily Trend
Line chart for Hourly Trend
Doughnut/Pie for % by Category/Size
Bars for Top/Bottom Sellers
Add Slicers/Timeline for order_date (Months/Years), pizza_category, and pizza_size.
Refresh
Use Refresh All to update pivots/charts when data changes.

7) Notes & Tips
If your SQL dialect differs (e.g., MySQL/Postgres), replace:
DATENAME(DW, order_date) with TO_CHAR(order_date, 'Day') or DAYNAME(order_date).
DATEPART(HOUR, order_time) with EXTRACT(HOUR FROM order_time) or HOUR(order_time).
In Excel, enable Add this data to the Data Model for distinct-count measures on order_id.
Keep currency/number formatting consistent with the dashboard (e.g., $, 2 decimals).
For month/quarter filters in SQL, reuse the examples near the end of the query set. 

8) Deliverables Checklist
 pizza_sales.csv cleaned and loaded
 All KPI queries executed & saved (sql/pizza_sales_queries.sql)
 Trend & mix queries executed
 Top/Bottom sellers extracted
 Excel dashboard with slicers & charts
 Optional: a PNG screenshot of the final dashboard in images/

9) License & Attribution
Use freely for learning and portfolio purposes.
SQL query structure adapted from the included PIZZA SALES SQL QUERIES.docx. 
