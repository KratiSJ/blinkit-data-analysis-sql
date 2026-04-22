# 🛒 Blinkit Sales Analysis using SQL

## 📌 Project Overview
This project focuses on analyzing Blinkit's sales data using SQL to uncover meaningful business insights related to sales performance, customer satisfaction, outlet distribution, and product trends.

The analysis includes:
- Data Cleaning
- KPI Analysis
- Sales Trend Analysis
- Outlet Performance
- Product Performance
- Conditional Aggregation
- Business Insights

The project demonstrates strong SQL skills including:
- Aggregate Functions
- CASE Statements
- COALESCE
- Window Functions
- GROUP BY Operations
- Data Cleaning Techniques

---

# 🎯 Business Objective

The main objective of this project is to conduct a comprehensive analysis of Blinkit's business data to identify:
- Total sales performance
- Customer rating trends
- Product category performance
- Outlet performance
- Impact of outlet size and location on sales
- Inventory distribution insights

---

# 🛠️ Tools & Technologies Used

- SQL
- PostgreSQL
- Data Analysis
- Data Cleaning
- Window Functions
- Aggregate Functions

---

# 📂 Dataset Information

The dataset contains information about:
- Item Type
- Fat Content
- Outlet Type
- Outlet Size
- Outlet Location
- Item Visibility
- Total Sales
- Customer Ratings
- Outlet Establishment Year

---

# 🗂️ Database Schema

```sql
CREATE TABLE blinkit_data (
    item_fat_content VARCHAR(50),
    item_identifier VARCHAR(50),
    item_type VARCHAR(50),
    outlet_establishment_year INT,
    outlet_identifier VARCHAR(50),
    outlet_location_type VARCHAR(50),
    outlet_size VARCHAR(50),
    outlet_type VARCHAR(50),
    item_visibility FLOAT,
    item_weight FLOAT,
    total_sales FLOAT,
    rating FLOAT
);
```

---

# 📥 Import Dataset

```sql
COPY blinkit_data
FROM 'C:\your_path\blinkit_data.csv'
DELIMITER ','
CSV HEADER;
```

---

# 🧹 Data Cleaning

The `item_fat_content` column contained inconsistent values like:
- LF
- low fat
- reg

These values were standardized for accurate analysis.

## SQL Query

```sql
UPDATE blinkit_data
SET item_fat_content =
CASE
    WHEN item_fat_content IN ('LF','low fat') THEN 'Low Fat'
    WHEN item_fat_content = 'reg' THEN 'Regular'
    ELSE item_fat_content
END;
```

---

# 📊 KPI Analysis

## 1️⃣ Total Sales

```sql
SELECT 
    CONCAT(CAST(SUM(total_sales)/1000000 AS DECIMAL(10,2)),'M') AS total_sales_millions
FROM blinkit_data;
```

## 2️⃣ Average Sales

```sql
SELECT 
    CAST(AVG(total_sales) AS DECIMAL(10,0)) AS avg_sales
FROM blinkit_data;
```

## 3️⃣ Total Orders

```sql
SELECT 
    COUNT(*) AS total_orders
FROM blinkit_data;
```

## 4️⃣ Average Rating

```sql
SELECT 
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data;
```

---

# 📈 Sales Analysis Queries

## 🔹 Sales by Fat Content

```sql
SELECT 
    item_fat_content,
    CONCAT(CAST(SUM(total_sales)/1000 AS DECIMAL(10,2)),'K') AS total_sales,
    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data
GROUP BY item_fat_content
ORDER BY total_sales DESC;
```

---

## 🔹 Top 5 Item Types

```sql
SELECT 
    item_type,
    CAST(SUM(total_sales) AS DECIMAL(10,0)) AS total_sales,
    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data
GROUP BY item_type
ORDER BY total_sales DESC
LIMIT 5;
```

---

## 🔹 Sales by Outlet Location (Fat Split)

```sql
SELECT 
    outlet_location_type,
    COALESCE(SUM(CASE 
        WHEN item_fat_content = 'Low Fat' 
        THEN total_sales 
    END),0) AS low_fat,

    COALESCE(SUM(CASE 
        WHEN item_fat_content = 'Regular' 
        THEN total_sales 
    END),0) AS regular

FROM blinkit_data
GROUP BY outlet_location_type
ORDER BY outlet_location_type;
```

### Key Concepts Used
- Conditional Aggregation
- CASE WHEN
- COALESCE Function
- Manual Pivoting

---

## 🔹 Sales by Establishment Year

```sql
SELECT 
    outlet_establishment_year,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,
    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data
GROUP BY outlet_establishment_year
ORDER BY total_sales DESC;
```

---

## 🔹 Sales Percentage by Outlet Size

```sql
SELECT 
    outlet_size,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,

    CAST(
        SUM(total_sales) * 100.0 /
        SUM(SUM(total_sales)) OVER()
    AS DECIMAL(10,2)) AS sales_percentage

FROM blinkit_data
GROUP BY outlet_size
ORDER BY total_sales DESC;
```

---

## 🔹 Sales by Outlet Location Type

```sql
SELECT 
    outlet_location_type,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,

    CAST(
        SUM(total_sales) * 100.0 /
        SUM(SUM(total_sales)) OVER()
    AS DECIMAL(10,2)) AS sales_percentage,

    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating

FROM blinkit_data
GROUP BY outlet_location_type
ORDER BY total_sales DESC;
```

---

## 🔹 Sales by Outlet Type

```sql
SELECT 
    outlet_type,

    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,

    CAST(
        SUM(total_sales) * 100.0 /
        SUM(SUM(total_sales)) OVER()
    AS DECIMAL(10,2)) AS sales_percentage,

    CAST(AVG(total_sales) AS DECIMAL(10,0)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,2)) AS avg_rating,
    CAST(AVG(item_visibility) AS DECIMAL(10,2)) AS avg_visibility

FROM blinkit_data
GROUP BY outlet_type
ORDER BY total_sales DESC;
```

---

# 📌 Key Insights

- Regular fat products generated higher sales compared to low-fat products.
- Certain product categories contributed significantly more revenue.
- Medium-sized outlets showed strong sales performance.
- Outlet location had a major impact on total sales.
- Customer ratings remained relatively stable across outlet types.

---

# 🚀 Project Highlights

✔ SQL-Based Data Analysis  
✔ Data Cleaning & Transformation  
✔ KPI Reporting  
✔ Window Functions  
✔ Conditional Aggregation  
✔ Business Insight Generation  
✔ Sales Performance Analysis  

---

# 📁 Project Structure

📦 Blinkit-SQL-Analysis
├── 📄 BlinkIT Grocery Data.csv
├── 📄 Blinkit Analysis.pptx
├── 📄 Query Doc.docx
├── 📄 blinkit_data.json
├── 📄 README.md
└── 📄 queries.sql

---

# 👨‍💻 Author

## Sachin Maurya

---

# ⭐ If you found this project useful, don't forget to star the repository!
