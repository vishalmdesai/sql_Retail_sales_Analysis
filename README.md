# sql_Retail_sales_Analysis

## Project Overview
Project Title: Retail Sales Analysis  
Level: Beginner  
Database: retail_sales_analysis  

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL


## Objectives
1] Set up a retail sales database: Create and populate a retail sales database with the provided sales data.  
2] Data Cleaning: Identify and remove any records with missing or null values.  
3] Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.  
4] Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.  


## Project Structure
1. Database Setup
• Database Creation: The project starts by creating a database named retail_sales_analysis.  
• Table Creation: A table named retail_sales is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE reatail_sales_analysis;
```

```sql
create table retail_sales_analysis(
transactions_id INT PRIMARY KEY,
sale_date DATE,
sale_time TIME,
customer_id INT,
gender VARCHAR(8),
age INT,
category VARCHAR(20),
quantity INT,
price_per_unit FLOAT,
cogs FLOAT,
total_sale FLOAT
);

SELECT * FROM retail_sales_analysis;
```

2. Data Exploration & Cleaning
• Record Count: Determine the total number of records in the dataset.  
• Customer Count: Find out how many unique customers are in the dataset.  
• Category Count: Identify all unique product categories in the dataset.  
• Null Value Check: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT * FROM retail_sales_analysis;

SELECT COUNT(*)AS TOTAL_SALE FROM retail_sales_analysis;

SELECT COUNT(DISTINCT(customer_id))AS UNIQUE_CUSTOMER FROM retail_sales_analysis;

SELECT DISTINCT(category) AS UNIQUE_CATEGORY FROM retail_sales_analysis;
```

```sql
SELECT * FROM retail_sales_analysis
WHERE transactions_id IS NULL

SELECT * FROM retail_sales_analysis
WHERE sale_date IS NULL


SELECT * FROM retail_sales_analysis
WHERE
    transactions_id IS NULL
	OR
	sale_date IS NULL
	OR
	sale_time IS NULL
	OR
	customer_id IS NULL
	OR
	gender IS NULL
	OR
	age IS NULL
	OR
	category IS NULL
	OR
	quantity IS NULL
	OR
	price_per_unit IS NULL
	OR
	cogs IS NULL
	OR
	total_sale IS NULL;


DELETE FROM retail_sales_analysis
WHERE
    transactions_id IS NULL
	OR
	sale_date IS NULL
	OR
	sale_time IS NULL
	OR
	customer_id IS NULL
	OR
	gender IS NULL
	OR
	age IS NULL
	OR
	category IS NULL
	OR
	quantity IS NULL
	OR
	price_per_unit IS NULL
	OR
	cogs IS NULL
	OR
	total_sale IS NULL;
	

SELECT count(*) FROM retail_sales_analysis;
```
## DATA ANALYSIS & BUSSINESS KEY PROBLEM'S & ANSWERS.

1.] Write a SQL query to retrieve all columns for sales made on '2022-11-05'
```sql
SELECT * FROM retail_sales_analysis
 where sale_date = '2022-11-05';
```
2.] Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is less than 4 in the month of Nov-2022:
```sql
SELECT * FROM retail_sales_analysis
WHERE category = 'Clothing'
and quantity < 4
and TO_CHAR(sale_date, 'YYYY-MM') = '2022-11';
```
3.] Write a SQL query to calculate the total sales (total_sale) for each category.
```sql
SELECT category,SUM(total_sale)AS Total_Amount 
FROM retail_sales_analysis 
GROUP BY category;
```
4.] Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.
```sql
SELECT category, ROUND(AVG(age),2) AS average_age 
from retail_sales_analysis 
WHERE category = 'Beauty' 
GROUP BY 1;
```
5.] Write a SQL query to find all transactions where the total_sale is greater than 1000.
```sql
SELECT * FROM retail_sales_analysis 
where total_sale > 1000;
```
6.] Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.
```sql
SELECT category,gender,
COUNT(transactions_id) AS Total_transaction 
FROM retail_sales_analysis 
GROUP BY category,gender
ORDER BY 1;
```
7.] Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.
```sql
SELECT * FROM retail_sales_analysis;

SELECT 
    YEAR,
    MONTH,
    AVG_Sale,
    rank
FROM (
    SELECT
        EXTRACT(YEAR FROM sale_date) AS YEAR,
        EXTRACT(MONTH FROM sale_date) AS MONTH,
        AVG(total_sale) AS AVG_Sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales_analysis
    GROUP BY EXTRACT(YEAR FROM sale_date), EXTRACT(MONTH FROM sale_date)
) AS t1
WHERE rank = 1;
```
8.] Write a SQL query to find the top 5 customers based on the highest total sales.
```sql
SELECT 
	customer_id,
	sum(total_sale)AS Total_sale
from retail_sales_analysis 
GROUP BY customer_id
ORDER BY Total_sale DESC
LIMIT 5;
```
9.] Write a SQL query to find the number of unique customers who purchased items from each category.
```sql
SELECT category,
	count(DISTINCT(customer_id)) ASUNIQUE_CUSTOMERS
FROM retail_sales_analysis
GROUP BY 1;
```
10.] Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)
```sql
WITH hourly_sales AS
(
SELECT *,
	CASE
		WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'MORNING'
		WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'AFTERNOON'
		ELSE 'EVENING'
	END AS SHIFT
FROM retail_sales_analysis
)
SELECT shift,count(*) AS Total_Orders
FROM hourly_sales
GROUP BY shift;
```
