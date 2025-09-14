# ☕ Cafe Sales Analysis — Power BI + SQL

![Report View](/Page%201.png)

## Introduction
An end-to-end **data analytics project** combining **SQL** for data cleaning & transformation and **Power BI** for interactive dashboarding.  
The project explores a cafe’s sales data to uncover trends in revenue, orders, product performance, and customer behavior.  

---

## 🚀 Project Snapshot
- **Total Sales (Feb 2023):** $76K ▼ -6.8% vs last month  
- **Total Orders (Feb 2023):** 16,359 ▼ -5.5% vs last month  
- **Total Quantity Sold (Feb 2023):** 23,550 ▼ -5.3% vs last month:contentReference[oaicite:2]{index=2}  
- **Overall (Jan–Feb 2023):** $698,812 sales | 149,116 orders | 214,470 items sold:contentReference[oaicite:3]{index=3}

---

## 📌 Key Power BI Features
- **KPI cards:** Track Total Sales, Orders, Quantity with MoM growth/decline.  
- **Sales by weekday vs. weekend:** Weekdays ($54K, ~71%) vs Weekends ($22K, ~29%).  
- **Store-level performance:** Hell's Kitchen (~$25.7K), Lower Manhattan (~$25.3K), Astoria (~$25.1K).  
- **Product category sales:** Coffee (~$29.3K), Tea (~$21.7K), Bakery (~$9K), Chocolate, Beans, Loose Tea, etc.  
- **Top products:** Barista Espresso (~$10K), Brewed Chai Tea (~$8.4K), Hot Chocolate (~$8.1K), Gourmet Coffee, etc.  
- **Hourly sales heatmap:** Identifies peak hours (8–10 AM, 2–3 PM).  
- **Trend analysis:** Daily/weekly sales variations with average daily sales ~$2.7K.  
- **Interactive visuals:** Filters for month, store, category, product, day, and hour.  

---

## 🛠 SQL Work
SQL was used for **data cleaning, preparation, and KPI calculation** before visualization in Power BI.  

### 🔧 Data Preparation Queries
- Convert `transaction_date` & `transaction_time` columns to proper `DATE` and `TIME` formats.  
- Cleaned column names and standardized data types.  

### 📊 Analytical Queries
- **Total Sales, Orders, Quantity** for a selected month.  
- **MoM growth %** using `LAG()` window function.  
- **Daily sales vs. average sales** → Above/Below Average classification.  
- **Sales by weekday vs. weekend** using `DAYOFWEEK()`.  
- **Sales by store location** ranking (Hell’s Kitchen, Manhattan, Astoria).  
- **Sales by category and product (Top 10)**.  
- **Hourly sales trends** across the week. 


### ✅ Example Query: Month-over-Month Sales Growth
```sql
SELECT 
    MONTH(transaction_date) AS month,
    ROUND(SUM(unit_price * transaction_qty)) AS total_sales,
    (SUM(unit_price * transaction_qty) - 
     LAG(SUM(unit_price * transaction_qty), 1) OVER (ORDER BY MONTH(transaction_date))) 
     / LAG(SUM(unit_price * transaction_qty), 1) OVER (ORDER BY MONTH(transaction_date)) * 100 
     AS mom_increase_percentage
FROM cafe_sales
WHERE MONTH(transaction_date) IN (4, 5)
GROUP BY MONTH(transaction_date)
ORDER BY MONTH(transaction_date);
 


