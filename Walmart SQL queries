-- Create database
CREATE DATABASE walmart;

\c walmart;  -- connect to walmart database

-- Create table
CREATE TABLE sales(
    invoice_id VARCHAR(30) PRIMARY KEY,
    branch VARCHAR(5) NOT NULL,
    city VARCHAR(30) NOT NULL,
    customer_type VARCHAR(30) NOT NULL,
    gender VARCHAR(10) NOT NULL,
    product_line VARCHAR(100) NOT NULL,
    unit_price NUMERIC(10,2) NOT NULL,
    quantity INT NOT NULL,
    vat NUMERIC(6,4) NOT NULL,
    total NUMERIC(12,4) NOT NULL,
    date TIMESTAMP NOT NULL,
    time TIME NOT NULL,
    payment VARCHAR(15) NOT NULL,
    cogs NUMERIC(10,2) NOT NULL,
    gross_margin_pct NUMERIC(11,9),
    gross_income NUMERIC(12,4),
    rating NUMERIC(2,1)
);

-- Import CSV (Postgres way)
COPY sales(invoice_id, branch, city, customer_type, gender, product_line, unit_price, quantity, vat, total, date, time, payment, cogs, gross_margin_pct, gross_income, rating)
FROM '/Users/mohammedshehbazdamkar/Downloads/WalmartSalesData.csv.csv'
DELIMITER ','
CSV HEADER;

------------------- Feature Engineering -----------------------------

-- 1. Time_of_day
ALTER TABLE sales ADD COLUMN time_of_day VARCHAR(20);

UPDATE sales
SET time_of_day = CASE
    WHEN time BETWEEN '00:00:00' AND '12:00:00' THEN 'Morning'
    WHEN time BETWEEN '12:01:00' AND '16:00:00' THEN 'Afternoon'
    ELSE 'Evening'
END;

-- 2. Day_name
ALTER TABLE sales ADD COLUMN day_name VARCHAR(10);

UPDATE sales
SET day_name = TO_CHAR(date, 'Day');

-- 3. Month_name
ALTER TABLE sales ADD COLUMN month_name VARCHAR(10);

UPDATE sales
SET month_name = TO_CHAR(date, 'Month');

----------------Exploratory Data Analysis (EDA)----------------------

-- 1. Distinct cities
SELECT DISTINCT city FROM sales;

-- 2. Branch and city
SELECT DISTINCT branch, city FROM sales;

-- Product Analysis
SELECT COUNT(DISTINCT product_line) FROM sales;

SELECT payment, COUNT(*) AS common_payment_method
FROM sales GROUP BY payment ORDER BY common_payment_method DESC LIMIT 1;

SELECT product_line, COUNT(*) AS most_selling_product
FROM sales GROUP BY product_line ORDER BY most_selling_product DESC LIMIT 1;

SELECT month_name, SUM(total) AS total_revenue
FROM sales GROUP BY month_name ORDER BY total_revenue DESC;

SELECT month_name, SUM(cogs) AS total_cogs
FROM sales GROUP BY month_name ORDER BY total_cogs DESC;

SELECT product_line, SUM(total) AS total_revenue
FROM sales GROUP BY product_line ORDER BY total_revenue DESC LIMIT 1;

SELECT city, SUM(total) AS total_revenue
FROM sales GROUP BY city ORDER BY total_revenue DESC LIMIT 1;

SELECT product_line, SUM(vat) AS VAT
FROM sales GROUP BY product_line ORDER BY VAT DESC LIMIT 1;

-- Product category
ALTER TABLE sales ADD COLUMN product_category VARCHAR(20);

UPDATE sales
SET product_category = CASE
    WHEN total >= (SELECT AVG(total) FROM sales) THEN 'Good'
    ELSE 'Bad'
END;

-- Branch with more than avg products
SELECT branch, SUM(quantity) AS quantity
FROM sales GROUP BY branch HAVING SUM(quantity) > (SELECT AVG(quantity) FROM sales);

-- Most common product line by gender
SELECT gender, product_line, COUNT(*) AS total_count
FROM sales GROUP BY gender, product_line ORDER BY total_count DESC;

-- Average rating per product line
SELECT product_line, ROUND(AVG(rating),2) AS average_rating
FROM sales GROUP BY product_line ORDER BY average_rating DESC;

-- Sales Analysis
SELECT day_name, time_of_day, COUNT(invoice_id) AS total_sales
FROM sales WHERE day_name NOT IN ('Sunday','Saturday')
GROUP BY day_name, time_of_day;

SELECT customer_type, SUM(total) AS total_sales
FROM sales GROUP BY customer_type ORDER BY total_sales DESC LIMIT 1;

SELECT city, SUM(vat) AS total_vat
FROM sales GROUP BY city ORDER BY total_vat DESC LIMIT 1;

SELECT customer_type, SUM(vat) AS total_vat
FROM sales GROUP BY customer_type ORDER BY total_vat DESC LIMIT 1;

-- Customer Analysis
SELECT COUNT(DISTINCT customer_type) FROM sales;

SELECT COUNT(DISTINCT payment) FROM sales;

SELECT customer_type, COUNT(*) AS common_customer
FROM sales GROUP BY customer_type ORDER BY common_customer DESC LIMIT 1;

SELECT customer_type, SUM(total) AS total_sales
FROM sales GROUP BY customer_type ORDER BY total_sales DESC LIMIT 1;

SELECT gender, COUNT(*) AS all_genders
FROM sales GROUP BY gender ORDER BY all_genders DESC LIMIT 1;

SELECT branch, gender, COUNT(*) AS gender_distribution
FROM sales GROUP BY branch, gender ORDER BY branch;

SELECT time_of_day, AVG(rating) AS average_rating
FROM sales GROUP BY time_of_day ORDER BY average_rating DESC LIMIT 1;

SELECT branch, time_of_day, AVG(rating) AS average_rating
FROM sales GROUP BY branch, time_of_day ORDER BY average_rating DESC;

SELECT day_name, AVG(rating) AS average_rating
FROM sales GROUP BY day_name ORDER BY average_rating DESC LIMIT 1;

SELECT branch, day_name, AVG(rating) AS average_rating
FROM sales GROUP BY branch, day_name ORDER BY average_rating DESC;
