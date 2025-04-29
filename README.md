# task-6
# 🛒 Sales Data Analysis

This repository contains a dataset and BigQuery SQL analysis focused on sales performance across products, regions, and time periods.

## 📁 Dataset Overview

The dataset includes sales transactions with details such as product category, region, sales representative, discounts, and unit pricing.

**Filename:** `salesdata.csv`
We use SQL in **Google BigQuery** to:

- Summarize revenue by **region**, **quarter**, and **sales rep**
- Compare **quarterly performance** (e.g., Q1 vs Q2 of 2023)
- Analyze trends across **months**, **sales channels**, and **customer types**

### 📌 Key Columns

| Column Name        | Description |
|--------------------|-------------|
| `Sale_Date`        | Date of sale (`YYYY-MM-DD`) |
| `Sales_Rep`        | Salesperson responsible for the transaction |
| `Region`           | Geographic region of the sale |
| `Sales_Amount`     | Total sale amount (may include discounts) |
| `Quantity_Sold`    | Number of units sold |
| `Unit_Price`       | Price per unit before discount |
| `Unit_Cost`        | Cost per unit |
| `Discount`         | Discount applied (e.g., `0.10` = 10%) |
| `Product_Category` | Category of product sold |
| `Customer_Type`    | Indicates whether the customer is New or Returning |
| `Sales_Channel`    | Sales medium (e.g., Online, Retail) |

## 💡 Calculation
SELECT 
  Sale_Date,
  EXTRACT(MONTH FROM Sale_Date) AS Sale_Month
FROM 
  `imposing-vista-456007-p4.sales.data`
LIMIT 20;
----
----
SELECT
  EXTRACT(YEAR FROM Sale_Date) AS Sale_Year,
  EXTRACT(MONTH FROM Sale_Date) AS Sale_Month,
  COUNT(*) AS Sales_Count
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
  Sale_Year,
  Sale_Month
ORDER BY 
  Sale_Year,
  Sale_Month
LIMIT 20;
---
---
SELECT
  FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_YearMonth,
  COUNT(*) AS Sales_Count
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
   Sale_YearMonth
ORDER BY 
  Sale_YearMonth
LIMIT 20;
----
----
SELECT
  EXTRACT(YEAR FROM Sale_Date) AS Sale_Year,
  EXTRACT(MONTH FROM Sale_Date) AS Sale_Month,
  SUM(Sales_Amount) AS Total_Sales_Amount
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
   Sale_Year,
  Sale_Month
ORDER BY 
  Sale_Year,
  Sale_Month
LIMIT 20;
-----
-----
SELECT
  FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_YearMonth,
  SUM(Sales_Amount) AS Total_Sales_Amount
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
   Sale_YearMonth
ORDER BY 
   Sale_YearMonth
LIMIT 20;
----
----
SELECT
  EXTRACT(YEAR FROM Sale_Date) AS Sale_Year,
  EXTRACT(MONTH FROM Sale_Date) AS Sale_Month,
  COUNT(DISTINCT product_id) AS Sales_Volume
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
   Sale_Year,
  Sale_Month
ORDER BY 
   Sale_Year,
  Sale_Month
LIMIT 20;
----
----
SELECT
  FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_YearMonth,
  COUNT(DISTINCT product_id) AS Sales_Volume
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
  Sale_YearMonth 
ORDER BY 
  Sale_YearMonth 
LIMIT 20;
----
----
SELECT
  FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_YearMonth,
  SUM(Sales_Amount) AS Total_Sales_Amount,
  COUNT(DISTINCT product_id) AS Sales_Volume
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
  Sale_YearMonth 
ORDER BY 
  Sale_YearMonth 
LIMIT 20;
----
----
SELECT
  FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_YearMonth,
  SUM(Sales_Amount) AS Total_Sales_Amount,
  COUNT(DISTINCT product_id) AS Sales_Volume,
  SUM(Quantity_Sold) AS Total_Units_Sold,
  SUM((Unit_Price - Unit_Cost) * Quantity_Sold) AS Total_Profit,
  AVG(Discount) AS Average_Discount
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
  Sale_YearMonth 
ORDER BY 
  Sale_YearMonth 
LIMIT 20;
----
----
SELECT
  FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_YearMonth,
  SUM(Sales_Amount) AS Total_Sales_Amount,
  COUNT(DISTINCT product_id) AS Sales_Volume,
  SUM(Quantity_Sold) AS Total_Units_Sold,
  SUM((Unit_Price - Unit_Cost) * Quantity_Sold) AS Total_Profit,
  AVG(Discount) AS Average_Discount
FROM 
  `imposing-vista-456007-p4.sales.data`
GROUP BY 
  Sale_YearMonth
ORDER BY 
  Total_Sales_Amount DESC,
  Sale_YearMonth ASC
LIMIT 20;
----
----
SELECT SUM(Unit_Price * (1 - Discount)) AS Total_Revenue
FROM imposing-vista-456007-p4.sales.data;
----
----
SELECT Region,
  SUM(Unit_Price * Quantity_Sold * (1 - Discount)) AS Total_Revenue
FROM imposing-vista-456007-p4.sales.data
GROUP BY Region;
----
----
SELECT Sales_Rep,
  SUM(Unit_Price * Quantity_Sold * (1 - Discount)) AS Total_Revenue
FROM imposing-vista-456007-p4.sales.data
GROUP BY Sales_Rep;
----
----
SELECT FORMAT_DATE('%Y-%m', DATE(Sale_Date)) AS Sale_Month,
  SUM(Unit_Price * Quantity_Sold * (1 - Discount)) AS Total_Revenue
FROM imposing-vista-456007-p4.sales.data
GROUP BY Sale_Month
ORDER BY Sale_Month;
-----
-----
SELECT Region,
  CONCAT('Q', CAST(EXTRACT(QUARTER FROM DATE(Sale_Date)) AS STRING)) AS Quarter,
  SUM(Unit_Price * Quantity_Sold * (1 - Discount)) AS Total_Revenue
FROM imposing-vista-456007-p4.sales.data
WHERE EXTRACT(YEAR FROM DATE(Sale_Date)) = 2023
  AND EXTRACT(QUARTER FROM DATE(Sale_Date)) IN (1, 2)
GROUP BY Region, Quarter
ORDER BY Region, Quarter
LIMIT 20;
-----
