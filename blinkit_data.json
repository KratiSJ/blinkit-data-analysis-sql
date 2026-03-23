-- ==============================
-- BLINKIT SALES ANALYSIS PROJECT
-- ==============================

-- 1. Create Table
DROP TABLE IF EXISTS blinkit_data;

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

-- 2. Load Data from CSV File (IMPORTANT STEP)
-- NOTE: Change file path according to your system

COPY blinkit_data
FROM 'C:\your_path\blinkit_data.csv'
DELIMITER ','
CSV HEADER;

-- Alternative (pgAdmin Import Option)
-- Right click table → Import/Export → Select CSV file → Enable Header → Import

-- 3. Data Cleaning
UPDATE blinkit_data
SET item_fat_content =
CASE
    WHEN item_fat_content IN ('LF','low fat') THEN 'Low Fat'
    WHEN item_fat_content = 'reg' THEN 'Regular'
    ELSE item_fat_content
END;

-- 4. Data Preview
SELECT * FROM blinkit_data;

-- 5. Basic Checks
SELECT COUNT(*) AS total_records FROM blinkit_data;
SELECT DISTINCT item_fat_content FROM blinkit_data;

-- ==============================
-- KPI ANALYSIS
-- ==============================

-- Total Sales
SELECT 
    CONCAT(CAST(SUM(total_sales)/1000000 AS DECIMAL(10,2)),'M') AS total_sales_millions
FROM blinkit_data
WHERE outlet_establishment_year = 2022;

-- Average Sales
SELECT 
    CAST(AVG(total_sales) AS DECIMAL(10,0)) AS avg_sales
FROM blinkit_data
WHERE outlet_establishment_year = 2022;

-- Number of Orders
SELECT 
    COUNT(*) AS total_orders
FROM blinkit_data
WHERE outlet_establishment_year = 2022;

-- Average Rating
SELECT 
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data;

-- ==============================
-- SALES ANALYSIS
-- ==============================

-- Sales by Fat Content
SELECT 
    item_fat_content,
    CONCAT(CAST(SUM(total_sales)/1000 AS DECIMAL(10,2)),'K') AS total_sales,
    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data
WHERE outlet_establishment_year = 2020
GROUP BY item_fat_content
ORDER BY total_sales DESC;

-- Top 5 Item Types
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

-- Sales by Outlet Location (Fat Split)
SELECT 
    outlet_location_type,
    COALESCE(SUM(CASE WHEN item_fat_content = 'Low Fat' THEN total_sales END),0) AS low_fat,
    COALESCE(SUM(CASE WHEN item_fat_content = 'Regular' THEN total_sales END),0) AS regular
FROM blinkit_data
GROUP BY outlet_location_type
ORDER BY outlet_location_type;

-- Sales by Establishment Year
SELECT 
    outlet_establishment_year,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,
    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data
GROUP BY outlet_establishment_year
ORDER BY total_sales DESC;

-- Sales % by Outlet Size
SELECT 
    outlet_size,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,
    CAST(SUM(total_sales) * 100.0 / SUM(SUM(total_sales)) OVER() AS DECIMAL(10,2)) AS sales_percentage
FROM blinkit_data
GROUP BY outlet_size
ORDER BY total_sales DESC;

-- Sales by Location Type
SELECT 
    outlet_location_type,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,
    CAST(SUM(total_sales) * 100.0 / SUM(SUM(total_sales)) OVER() AS DECIMAL(10,2)) AS sales_percentage,
    CAST(AVG(total_sales) AS DECIMAL(10,1)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,1)) AS avg_rating
FROM blinkit_data
GROUP BY outlet_location_type
ORDER BY total_sales DESC;

-- Sales by Outlet Type
SELECT 
    outlet_type,
    CAST(SUM(total_sales) AS DECIMAL(10,2)) AS total_sales,
    CAST(SUM(total_sales) * 100.0 / SUM(SUM(total_sales)) OVER() AS DECIMAL(10,2)) AS sales_percentage,
    CAST(AVG(total_sales) AS DECIMAL(10,0)) AS avg_sales,
    COUNT(*) AS total_items,
    CAST(AVG(rating) AS DECIMAL(10,2)) AS avg_rating,
    CAST(AVG(item_visibility) AS DECIMAL(10,2)) AS avg_visibility
FROM blinkit_data
GROUP BY outlet_type
ORDER BY total_sales DESC;