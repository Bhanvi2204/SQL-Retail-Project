# SQL-Retail-Project

# 🛒 MySQL Retail Sales Analysis Project

## 📌 Project Overview

This is a beginner-friendly **MySQL project** focused on analyzing a retail sales dataset. It includes data cleaning, exploration, and querying to extract meaningful business insights such as top customers, sales by category, purchasing patterns by time of day, and more.

---

## 🧰 Database & Table Setup

### 📂 Database Created:
```sql
CREATE DATABASE mysql_project;
USE mysql_project;
```

### 📊 Table: `retail_sales`
```sql
CREATE TABLE retail_sales (
  transactions_id   INT PRIMARY KEY,
  sale_date         DATE,
  sale_time         TIME,
  customer_id       INT,
  gender            VARCHAR(15),
  age               INT,
  category          VARCHAR(15),
  quantity          INT,
  price_per_unit    FLOAT,
  cogs              FLOAT,
  total_sale        FLOAT
);
```

---

## 🧹 Data Cleaning

Checked for nulls and deleted any rows with missing values:
```sql
DELETE FROM retail_sales 
WHERE transactions_id IS NULL OR ... OR total_sale IS NULL;
```

---

## 🔍 Data Exploration & SQL Queries

### ✅ Q1. All sales made on '2022-11-05':
```sql
SELECT * FROM retail_sales WHERE sale_date = '2022-11-05';
```

### ✅ Q2. Clothing category sales with quantity = 4 in Nov 2022:
***sql
SELECT * 
FROM retail_sales
WHERE category = 'clothing' AND quantity = 4 AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11';
***

### ✅ Q3. Total sales for each category:
```sql
SELECT category, SUM(total_sale) FROM retail_sales GROUP BY category;
```

### ✅ Q4. Average age of customers in 'Beauty' category:
```sql
SELECT ROUND(AVG(age), 2) FROM retail_sales WHERE category = 'Beauty';
```

### ✅ Q5. Transactions where total sale > 1000:
```sql
SELECT * FROM retail_sales WHERE total_sale > 1000;
```

### ✅ Q6. Number of transactions by gender per category:
```sql
SELECT category, gender, COUNT(transactions_id) FROM retail_sales GROUP BY category, gender;
```

### ✅ Q7. Best-selling month (highest average sale) in each year:
```sql
SELECT year, month, avg_monthly_sales
FROM (
  SELECT 
    YEAR(sale_date) AS year,
    MONTH(sale_date) AS month,
    AVG(total_sale) AS avg_monthly_sales,
    RANK() OVER (PARTITION BY YEAR(sale_date) ORDER BY AVG(total_sale) DESC) AS rnk
  FROM retail_sales
  GROUP BY YEAR(sale_date), MONTH(sale_date)
) ranked
WHERE rnk = 1;
```

### ✅ Q8. Top 5 customers by total sales:
```sql
SELECT customer_id, SUM(total_sale) AS total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5;
```

### ✅ Q9. Unique customers per category:
```sql
SELECT category, COUNT(DISTINCT customer_id) FROM retail_sales GROUP BY category;
```

### ✅ Q10. Number of orders by shift (Morning, Afternoon, Evening):
```sql
SELECT 
  CASE
    WHEN HOUR(sale_time) < 12 THEN 'Morning'
    WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
    ELSE 'Evening'
  END AS shift,
  COUNT(*) AS order_count
FROM retail_sales
GROUP BY shift;
```

---

## 🎉 Conclusion

> ✅ **HURRAH! I HAVE SUCCESSFULLY COMPLETED MY FIRST MYSQL PROJECT**

- Practiced SQL queries with real-world data
- Performed data cleaning, aggregation, and time-based analysis
- Built a solid foundation for business intelligence projects using SQL

---

## 🧠 Next Steps (Optional)
- Visualize these results using Power BI or Tableau
- Build a dashboard with key KPIs like revenue per category, daily sales, customer retention

---
