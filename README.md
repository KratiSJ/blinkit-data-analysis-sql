# 🛒 Blinkit Sales Analysis (SQL Project)

## 📌 Project Overview
This project analyzes Blinkit retail sales data using PostgreSQL and pgAdmin.  
The objective is to extract meaningful business insights related to sales performance, product categories, and outlet-level trends.

---

## 🛠 Tools & Technologies
- PostgreSQL
- pgAdmin
- SQL

---

## 📂 Dataset Description
- Item Details (Fat Content, Item Type, Weight)
- Outlet Details (Location, Size, Type)
- Sales Data (Total Sales, Ratings)

---

## ⚙️ Project Workflow

### 1. Table Creation
Created structured table using SQL.

### 2. Data Loading
Loaded dataset from CSV using COPY command.

### 3. Data Cleaning
Standardized values in item_fat_content column.

### 4. Data Validation
Used SELECT, COUNT, DISTINCT to verify data.

---

## 📊 Key Analysis
- Total Sales
- Average Sales
- Total Orders
- Average Rating
- Sales by Item Type
- Sales by Outlet Type
- Sales by Location & Size

---

## 📈 Key Insights
- Top outlet types generate highest revenue
- Low Fat products perform well
- Larger outlets have higher sales
- Sales vary by location
- Higher ratings → better sales

---

## 📁 Project Structure

blinkit-sales-analysis/
│
├── data/
│   └── blinkit_data.csv
│
├── sql/
│   └── blinkit_analysis.sql
│
└── README.md

---

## 🚀 How to Run
1. Open pgAdmin
2. Create database
3. Run SQL file
4. Import CSV
5. Execute queries

---

## 💼Highlight
- Data cleaning using SQL
- Data analysis using aggregations
- Business insights generation

---

## 👨‍💻 Author
Krati Jain
