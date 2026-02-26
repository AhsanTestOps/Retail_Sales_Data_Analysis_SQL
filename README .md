# 🛒 Retail Sales Data Analysis — SQL

![SQL](https://img.shields.io/badge/SQL-PostgreSQL-blue?style=flat&logo=postgresql)
![Level](https://img.shields.io/badge/Level-Beginner-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

## 📌 Project Overview

**Project Title**: Retail Sales Analysis  
**Level**: Beginner  
**Database**: `Retail Sales Analysis SQL Project`

This project showcases my SQL skills by exploring, cleaning, and analyzing retail sales data. I set up a retail sales database from scratch, performed exploratory data analysis (EDA), and answered real business questions using SQL queries. This project is part of my data analyst portfolio and reflects my ability to work with structured data end-to-end.

---

## 🎯 Objectives

1. **Database Setup** — Create and populate a retail sales database with real sales data.
2. **Data Cleaning** — Identify and remove records with missing or null values.
3. **Exploratory Data Analysis (EDA)** — Understand the dataset through basic statistical queries.
4. **Business Analysis** — Answer business-driven questions and extract meaningful insights.

---

## 🗂️ Project Structure

### 1. Database Setup

Created a database with a `retail_sales` table containing transaction-level data.

```sql
CREATE DATABASE retail_sales_db;

CREATE TABLE retail_sales
(
    transactions_id   INT PRIMARY KEY,
    sale_date         DATE,
    sale_time         TIME,
    customer_id       INT,
    gender            VARCHAR(10),
    age               INT,
    category          VARCHAR(35),
    quantity          INT,
    price_per_unit    FLOAT,
    cogs              FLOAT,
    total_sale        FLOAT
);
```

---

### 2. Data Exploration & Cleaning

```sql
-- Total records
SELECT COUNT(*) FROM retail_sales;

-- Unique customers
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;

-- Unique product categories
SELECT DISTINCT category FROM retail_sales;

-- Check for NULLs
SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

-- Remove NULL records
DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;
```

---

### 3. Business Analysis & SQL Queries

**Q1. Retrieve all sales made on '2022-11-05'**
```sql
SELECT * FROM retail_sales
WHERE sale_date = '2022-11-05';
```

**Q2. Clothing transactions with quantity ≥ 4 in November 2022**
```sql
SELECT * FROM retail_sales
WHERE category = 'Clothing'
  AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
  AND quantity >= 4;
```

**Q3. Total sales per category**
```sql
SELECT 
    category,
    SUM(total_sale) AS net_sale,
    COUNT(*) AS total_orders
FROM retail_sales
GROUP BY 1;
```

**Q4. Average age of customers in the 'Beauty' category**
```sql
SELECT ROUND(AVG(age), 2) AS avg_age
FROM retail_sales
WHERE category = 'Beauty';
```

**Q5. Transactions where total sale > 1000**
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000;
```

**Q6. Total transactions by gender per category**
```sql
SELECT 
    category,
    gender,
    COUNT(*) AS total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY 1;
```

**Q7. Best-selling month in each year (by average sales)**
```sql
SELECT year, month, avg_sale
FROM (
    SELECT 
        EXTRACT(YEAR FROM sale_date) AS year,
        EXTRACT(MONTH FROM sale_date) AS month,
        AVG(total_sale) AS avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) AS rank
    FROM retail_sales
    GROUP BY 1, 2
) AS t1
WHERE rank = 1;
```

**Q8. Top 5 customers by total sales**
```sql
SELECT 
    customer_id,
    SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

**Q9. Unique customers per category**
```sql
SELECT 
    category,
    COUNT(DISTINCT customer_id) AS unique_customers
FROM retail_sales
GROUP BY category;
```

**Q10. Orders by shift (Morning / Afternoon / Evening)**
```sql
WITH hourly_sale AS (
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END AS shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

---

## 📊 Key Findings

- **Customer Demographics** — Customers span various age groups, with purchases across Clothing and Beauty categories.
- **High-Value Transactions** — Multiple transactions exceeded $1,000, indicating premium buying behavior.
- **Sales Trends** — Monthly analysis reveals seasonal peaks that can inform inventory and marketing strategies.
- **Top Customers** — A small group of customers contributes significantly to total revenue.
- **Shift Analysis** — Orders are distributed across Morning, Afternoon, and Evening shifts, useful for staffing decisions.

---

## 🛠️ Tools & Technologies

- **SQL** — PostgreSQL
- **Database** — Retail Sales Analysis SQL Project
- **GitHub** — Version Control & Portfolio

---

## 📁 Repository Structure

```
📁 Retail_Sales_Data_Analysis_SQL/
   ├── Retail Sales Analysis SQL Project.sql   ← All SQL queries
   ├── SQL - Retail Sales Analysis_utf.csv     ← Raw dataset
   └── README.md                               ← Project documentation
```

---

## 🚀 How to Run

1. Clone this repository
   ```bash
   git clone https://github.com/AhsanTestOps/Retail_Sales_Data_Analysis_SQL.git
   ```
2. Open your SQL client (e.g., pgAdmin, DBeaver)
3. Import the dataset `SQL - Retail Sales Analysis_utf.csv` into your database
4. Open and run `Retail Sales Analysis SQL Project.sql` to create the database, clean the data, and execute all analysis queries

---

## 👤 Author

**Ahsan**  
Data Analyst | SQL Engineer

Feel free to reach out if you have any questions or feedback about this project!

---

⭐ *If you found this project helpful, consider giving it a star!*
