# 🍕 Pizza Sales SQL Analysis

## 📌 Project Overview
This project analyzes pizza sales data using SQL to extract business insights. 
It is divided into Basic, Intermediate, and Advanced levels to demonstrate SQL proficiency.

---

## 🛠️ Tools & Technologies
- SQL (MySQL / PostgreSQL / SQL Server)
- Relational Database
- Data Analysis

---

## 📂 Dataset Description
Tables used:
- orders (order_id, date, time)
- order_details (order_id, pizza_id, quantity)
- pizzas (pizza_id, size, price, pizza_type_id)
- pizza_types (pizza_type_id, name, category)

---

## 🔹 Basic Analysis

1. Total Number of Orders
SELECT COUNT(DISTINCT order_id) AS total_orders FROM orders;

2. Total Revenue Generated
SELECT SUM(od.quantity * p.price) AS total_revenue
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id;

3. Highest Priced Pizza
SELECT pizza_id, price
FROM pizzas
ORDER BY price DESC
LIMIT 1;

4. Most Common Pizza Size Ordered
SELECT p.size, COUNT(*) AS count
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
GROUP BY p.size
ORDER BY count DESC
LIMIT 1;

5. Top 5 Most Ordered Pizza Types
SELECT pt.name, SUM(od.quantity) AS total_quantity
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY total_quantity DESC
LIMIT 5;

---

## 🔸 Intermediate Analysis

1. Total Quantity by Pizza Category
SELECT pt.category, SUM(od.quantity) AS total_quantity
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category;

2. Orders Distribution by Hour
SELECT HOUR(time) AS order_hour, COUNT(*) AS total_orders
FROM orders
GROUP BY order_hour
ORDER BY order_hour;

3. Category-wise Distribution
SELECT pt.category, COUNT(*) AS total_orders
FROM pizza_types pt
JOIN pizzas p ON pt.pizza_type_id = p.pizza_type_id
JOIN order_details od ON p.pizza_id = od.pizza_id
GROUP BY pt.category;

4. Average Pizzas Ordered Per Day
SELECT AVG(daily_count) AS avg_pizzas_per_day
FROM (
    SELECT o.date, SUM(od.quantity) AS daily_count
    FROM orders o
    JOIN order_details od ON o.order_id = od.order_id
    GROUP BY o.date
) AS daily_orders;

5. Top 3 Pizza Types Based on Revenue
SELECT pt.name, SUM(od.quantity * p.price) AS revenue
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY revenue DESC
LIMIT 3;

---

## 🔺 Advanced Analysis

1. Percentage Contribution to Total Revenue
SELECT pt.name,
       SUM(od.quantity * p.price) AS revenue,
       (SUM(od.quantity * p.price) /
        (SELECT SUM(od.quantity * p.price)
         FROM order_details od
         JOIN pizzas p ON od.pizza_id = p.pizza_id)) * 100 AS percentage
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY percentage DESC;

2. Cumulative Revenue Over Time
SELECT o.date,
       SUM(SUM(od.quantity * p.price)) OVER (ORDER BY o.date) AS cumulative_revenue
FROM orders o
JOIN order_details od ON o.order_id = od.order_id
JOIN pizzas p ON od.pizza_id = p.pizza_id
GROUP BY o.date;

3. Top 3 Pizza Types by Revenue (Per Category)
SELECT category, name, revenue
FROM (
    SELECT pt.category, pt.name,
           SUM(od.quantity * p.price) AS revenue,
           RANK() OVER (PARTITION BY pt.category ORDER BY SUM(od.quantity * p.price) DESC) AS rank
    FROM order_details od
    JOIN pizzas p ON od.pizza_id = p.pizza_id
    JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
    GROUP BY pt.category, pt.name
) ranked
WHERE rank <= 3;

---

## 📊 Key Insights
- Identifies top-selling pizzas
- Analyzes revenue contribution
- Shows customer ordering patterns
- Helps in business decision making

---

## 🚀 Conclusion
This project demonstrates:
- SQL Joins
- Aggregations
- Window Functions
- Subqueries
- Business Analysis Skills
