# Retail Sales Database Project

**Overview**
This project focuses on analyzing retail sales data by running a series of SQL queries to extract insights from the dataset stored in the retail_sales_db database. The project covers data exploration, cleaning, and analysis of various aspects of the sales data, including customer behavior, sales trends, and category-specific analysis.

**Database Details**

Database Name: retail_sales_db

**Table: sales**
The sales table contains the following columns:

transactions_id: Unique identifier for each transaction.

sale_date: Date of the sale.

sale_time: Time when the sale occurred.

customer_id: Unique identifier for each customer.

gender: Gender of the customer.

age: Age of the customer.

category: Category of the product sold (e.g., clothing, beauty, etc.).

quantity: Number of units sold.

price_per_unit: Price per unit of the product sold.

cogs: Cost of goods sold.

total_sale: Total sale amount for the transaction.

**Objectives**

Import and clean the sales data by identifying and handling any missing values.

Explore the data to extract insights into the number of sales, unique customers, and trends in different product categories.

Perform analysis to answer specific business questions using SQL queries.

**SQL Queries**

**Data Cleaning**

```
SELECT COUNT(*) FROM sales
```
USE retail_sales_db;
SELECT * FROM sales;

-- **Checking for the all the rows imported**
```
SELECT COUNT(*) FROM sales;
```
-- **Checking for Null values**
```
SELECT * 
FROM sales
WHERE '﻿transactions_id' IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```

-- **Delete Null values if it is available**
```
DELETE FROM sales
WHERE '﻿transactions_id' IS NULL
   OR sale_date IS NULL
   OR sale_time IS NULL
   OR customer_id IS NULL
   OR gender IS NULL
   OR age IS NULL
   OR category IS NULL
   OR quantiy IS NULL
   OR price_per_unit IS NULL
   OR cogs IS NULL
   OR total_sale IS NULL;
```
   
   # Data Exploration
   -- **How Many Sales we have **
   ```
   SELECT COUNT(*) AS Total_sale FROM sales;
```
   
   **How Many unique customers we have**
   ```
   SELECT COUNT(customer_id) AS Total_sale FROM sales;
   ```
-- **Q.1 Write a Query to retrive all columns for sales made on "2022-11-05"**
```
SELECT * FROM sales
WHERE sale_date='2022-11-05';
```
![Q1](https://github.com/user-attachments/assets/88ef26b8-69e5-409f-bf38-d6cf9d027742)

-- **Q.2 Write a Query to retrive all transections where the category is "clothing" and the quantity sold is more than 10 in the month of nov-22**
```
SELECT * 
FROM sales
WHERE category = 'clothing' 
  AND quantiy >= 4
  AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11';
  ```
![Q2 Output](https://github.com/user-attachments/assets/13f2bc73-b734-4506-a9dc-87ec7c712e79)

-- **Q.3 Write a SQL Query to calculate total sales in each catergory**
```
SELECT 
category,
SUM(total_sale) as net_sale,
COUNT(*) AS total_orders
FROM sales
GROUP BY 1;
```
![Q3 Output](https://github.com/user-attachments/assets/14c218a5-73d4-4c13-844b-d58f8090ef25)

-- **Q.4 Write a SQL Query to find the Average age of customers who purchased items from the "Beauty" Category.**
```
SELECT 
ROUND(AVG(age),2) as avg_age
FROM sales
WHERE category ='Beauty';
```
![Q4 Output](https://github.com/user-attachments/assets/5f9aa008-166a-486b-a4f2-a7a2d73537a6)

-- **Q.5 Write a SQL query to find all transections where the total_sale > 1000**
```
SELECT * FROM sales
WHERE total_sale > 1000;
```
![Q5 Output](https://github.com/user-attachments/assets/58a9f893-e4f5-435f-be4f-663a13847915)

-- **Q.6 Write a SQL Query to find the total number of transections (transection_ID) made by each gender in each category.**
```
SELECT 
category,
gender,
count(*) as total_trans
FROM sales
GROUP BY 
category,
gender
ORDER BY 1;
```
![Q6 Output](https://github.com/user-attachments/assets/0d197e61-3853-4182-8dc0-efad6484b052)

--**Q.7 Write a SQL query to calculate the average sale for each month . find out best selling month in each year.**
```
SELECT 
    year,
    month,
    avg_sale
FROM
(
    SELECT 
        YEAR(sale_date) AS year,
        MONTH(sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER (PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) AS r
    FROM 
        sales
    GROUP BY 
        YEAR(sale_date), MONTH(sale_date)
) AS t1
WHERE r=1;
```
![Q7 Output](https://github.com/user-attachments/assets/19405b29-4215-484e-a2f6-4ecefe28216d)

-- **Q.8 Write a SQL Query to find the top 5 customers based on the highest total sales**
```
 SELECT 
 customer_id,
 SUM(total_sale) as total_sales
 FROM sales
 GROUP BY customer_id
 ORDER BY total_sales DESC
 LIMIT 5;
```
![Q8 Output](https://github.com/user-attachments/assets/90a91476-67b5-439d-9835-0ef50867ce2d)

-- **Q.9 Write a SQL Query to find the number of unique customers who purchased items from each category**
```
SELECT 
 category,
 COUNT(DISTINCT customer_id) AS Uniques_customers
 FROM sales
 GROUP BY category;
```
![Q9 Output](https://github.com/user-attachments/assets/3174d29b-9ad5-4368-936d-648b5f580ed5)

-- **Q.10 Write a SQL query to create each shift and number of orders (Example Morning <=12 & 17, Evening>17**
```
WITH Hourly_Sale
AS
(
SELECT *,
CASE
WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN "Morning"
WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN "Afternoon"
ELSE "Everything"
END as shift
FROM sales
)
SELECT
COUNT(*) As total_orders
FROM Hourly_Sale
GROUP BY shift;
```
![Q10 Output](https://github.com/user-attachments/assets/4aad3299-777c-4373-bed8-66caf24750d5)
























