# SQL_Retail_Sales_Project-1
Retail Sales Analysis SQL Project
Project Overview
Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

Objectives
Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
Data Cleaning: Identify and remove any records with missing or null values.
Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.
Project Structure
1. Database Setup
   Project Structure
Database Creation: The project starts by creating a database named Retailsales_db.

3.  Data Exploration & Cleaning
Record Count: Determine the total number of records in the dataset.
Customer Count: Find out how many unique customers are in the dataset.
Category Count: Identify all unique product categories in the dataset.
Null Value Check: Check for any null values in the dataset and delete records with missing data.


---Retrive the howl data 

    SELECT * FROM Retailsales;

--- Data Exploration & Cleaning

    SELECT *
    FROM Retailsales
    WHERE transactions_id IS NULL;

    SELECT * 
    FROM Retailsales
    WHERE sale_date IS NULL;

    SELECT *
    FROM Retailsales
    WHERE sale_time IS NULL OR customer_id IS NULL
    OR gender IS NULL OR age IS NULL
    OR category IS NULL OR  quantiy IS NULL
    OR price_per_unit IS NULL OR  cogs IS NULL;



---DELETE NULL VALUES FROM THE TABLE 

     DELETE FROM Retailsales
     WHERE sale_time IS NULL OR customer_id IS NULL
     OR gender IS NULL OR age IS NULL
     OR category IS NULL OR  quantiy IS NULL
     OR price_per_unit IS NULL OR  cogs IS NULL;


---Data Exploaration

--How many sales we have?

    SELECT COUNT (*) AS Total_Sales
    FROM Retailsales;

--How many unique cutomers we have ?

    SELECT Count   (DISTINCT customer_id) 
    <img width="500" height="500" alt="download" src="https://github.com/user-attachments/assets/11c379d2-e6cc-4bf3-9ea8-3cb6a714186a" />
    FROM Retailsales;

---How many ccategories we have ?

    SELECT COUNT(DISTINCT category)
    FROM Retailsales;

 3. Data Analysis & Findings
The following SQL queries were developed to answer specific business questions:

--Data Analysis & Findings

---Write a SQL query to retrieve all columns for sales made on '2022-11-05':

    SELECT *
    FROM Retailsales
    WHERE sale_date='2022-11-05';

 ---Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:

    SELECT *
    FROM Retailsales
    WHERE
    category ='Clothing'
    AND 
    quantiy >=4
    AND 
    sale_date >='2022-11-01'
    AND
    sale_date <='2022-12-01';

 ---Write a SQL query to calculate the total sales (total_sale) for each category

    SELECT category, COUNT (total_sale)  AS Total_Sales
    FROM Retailsales
    GROUP BY category;

 --- Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category

    SELECT ROUND (AVG (age),2) AS AVG_Age,category
    FROM Retailsales
    WHERE category = 'Beauty'
    GROUP BY category;

 ---Write a SQL query to find all transactions where the total_sale is greater than 1000.

    SELECT *
    FROM Retailsales
    WHERE total_sale >1000;

 ---Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category

    SELECT SUM(transactions_id) AS Total_transaction_id, category,gender 
    FROM  Retailsales
    GROUP BY category,gender ;

 ---Write a SQL query to calculate the average sale for each month. Find out best selling month in each year:
 
 


     WITH AVG_sales_Month AS 
    (
    SELECT 
    YEAR(sale_date)  AS  salesyear ,
    MONTH(sale_date) AS salesmonth,
    AVG(total_sale) AS AVG_Total_sales
    FROM Retailsales
    GROUP BY 
    YEAR(sale_date),
    MONTH(sale_date) 

    )
 
    , Ranksales  AS 

    (
    SELECT 
    salesyear,
    salesmonth,
    AVG_Total_sales,

    RANK()  OVER 
    (PARTITION BY salesyear 
    ORDER BY  AVG_Total_sales DESC
    )
    AS RNK
    FROM AVG_sales_Month
    ) 
    SELECT salesmonth,salesyear, AVG_Total_sales
    FROM Ranksales
    WHERE RNK=1
    ORDER BY salesyear ;

--- Write a SQL query to find the top 5 customers based on the highest total sales 

    SELECT TOP 5
    customer_id, 
    SUM (total_sale) AS totalsales
    FROM Retailsales
    GROUP BY customer_id
    ORDER BY totalsales DESC;

---Write a SQL query to find the number of unique customers who purchased items from each category.:

        SELECT category, Count ( DISTINCT customer_id)  AS Count_Customers
        FROM Retailsales 
        GROUP BY category;


---Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):

     with Hourly_sale AS 

      (
     SELECT  sale_time , 

     CASE 
    WHEN DATEPART(HOUR, sale_time) <12 THEN 'Morning'
    WHEN DATEPART(HOUR ,sale_time)  Between 12 AND  17 THEN 'Afternoon'
    ELSE 'Evening'
    END AS Shift
    FROM Retailsales

    )
    SELECT Shift,COUNT(*) AS total_orders
    FROM Hourly_sale
    GROUP BY Shift
    ORDER BY Shift












