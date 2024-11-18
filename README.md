# Flipkart E-commerce SQL Project

![Project Banner Placeholder](https://www.google.com/imgres?q=flipkart%20logo%204k&imgurl=https%3A%2F%2Fi.pinimg.com%2Foriginals%2F15%2Faa%2F16%2F15aa1678d4ee5615c5c53ed5c9968126.png&imgrefurl=https%3A%2F%2Fin.pinterest.com%2Fpin%2F192106740344173391%2F&docid=ItjkWfnGjy2xaM&tbnid=lN2lW8BsFy_nxM&vet=12ahUKEwiomY-HpOaJAxVBh1YBHdB8KHgQM3oECBUQAA..i&w=400&h=399&hcb=2&ved=2ahUKEwiomY-HpOaJAxVBh1YBHdB8KHgQM3oECBUQAA)

Welcome to my SQL project, where I analyze real-time data from **Flipkart**! This project uses a dataset of **20,000+ sales records** and additional tables for payments, products, and shipping data to explore and analyze e-commerce transactions, product sales, and customer interactions. The project aims to solve business problems through SQL queries, helping Flipkart make informed decisions.

## Table of Contents
- [Introduction](#introduction)
- [Project Structure](#project-structure)
- [Database Schema](#database-schema)
- [Business Problems](#business-problems)
- [SQL Queries & Analysis](#sql-queries--analysis)
- [Getting Started](#getting-started)
- [Questions & Feedback](#questions--feedback)
- [Contact Me](#contact-me)

---

## Introduction

This project demonstrates essential SQL skills by analyzing e-commerce data from **Flipkart**, focusing on sales, payments, products, and customer data. Through SQL, we answer critical business questions, uncover trends, and derive actionable insights that help improve business strategies and customer experiences. The project covers different SQL techniques including **Joins**, **Group By**, **Aggregations**, and **Date Functions**.

## Project Structure

1. **SQL Scripts**: Contains code to create the database schema and write queries for analysis.
2. **Dataset**: Real-time data representing e-commerce transactions, product details, customer information, and shipping status.
3. **Analysis**: SQL queries crafted to solve business problems, each focusing on understanding e-commerce sales and performance.

---

## Database Schema

Here’s an overview of the database structure:

### 1. **Customers Table**
- **customer_id**: Unique identifier for each customer
- **customer_name**: Name of the customer
- **state**: Location (state) of the customer

### 2. **Products Table**
- **product_id**: Unique identifier for each product
- **product_name**: Name of the product
- **price**: Price of the product
- **cogs**: Cost of goods sold
- **category**: Category of the product
- **brand**: Brand name of the product

### 3. **Sales Table**
- **order_id**: Unique order identifier
- **order_date**: Date the order was placed
- **customer_id**: Linked to the `customers` table
- **order_status**: Status of the order (e.g., Completed, Cancelled)
- **product_id**: Linked to the `products` table
- **quantity**: Quantity of products sold
- **price_per_unit**: Price per unit of the product

### 4. **Payments Table**
- **payment_id**: Unique payment identifier
- **order_id**: Linked to the `sales` table
- **payment_date**: Date the payment was made
- **payment_status**: Status of the payment (e.g., Payment Successed, Payment Failed)

### 5. **Shippings Table**
- **shipping_id**: Unique shipping identifier
- **order_id**: Linked to the `sales` table
- **shipping_date**: Date the order was shipped
- **return_date**: Date the order was returned (if applicable)
- **shipping_providers**: Shipping provider (e.g., Ekart, Bluedart)
- **delivery_status**: Status of delivery (e.g., Delivered, Returned)

## Business Problems

The following queries were created to solve specific business questions. Each query is designed to provide insights based on sales, payments, products, and customer data.

### Easy 
1. Retrieve a list of all customers with their corresponding product names they ordered (use an INNER JOIN between customers and sales tables).
```sql
SELECT 
c.customer_name,
p.product_name
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
INNER JOIN products AS p
ON p.product_id = s.product_id;
```
2. List all products and show the details of customers who have placed orders for them. Include products that have no orders (use a LEFT JOIN between products and sales tables).
```sql
SELECT 
p.product_name,
c.customer_name
FROM products AS p
LEFT JOIN sales AS s
ON p.product_id = s.product_id
LEFT JOIN customers AS c
ON c.customer_id = s.customer_id;
```
3. List all orders and their shipping status. Include orders that do not have any shipping records (use a LEFT JOINbetween sales and shippings tables).
```sql
SELECT
s.order_id,
s.customer_id,
sh.shipping_id,
sh.shipping_date,
sh.delivery_status
FROM sales AS s
LEFT JOIN shipping AS sh
ON s.order_id = sh.order_id;
```
4. Retrieve all products, including those with no orders, along with their price. Use a RIGHT JOIN between the products and sales tables.
```sql
SELECT 
p.product_name,
p.price,
s.order_status
FROM sales AS s
RIGHT JOIN products AS p
ON p.product_id = s.product_id;
```
5.Get a list of all customers who have placed orders, including those with no payment records. Use a FULL OUTER JOIN between the customers and payments tables.
```sql
SELECT
c.customer_name,
p.payment_status
FROM customers AS c
FULL OUTER JOIN sales AS s
ON c.customer_id = s.customer_id
FULL OUTER JOIN payments AS p
ON p.order_id = s.order_id;
```
   
### Medium to Hard
1. `Find the total number of completed orders made by customers from the state 'Delhi' (use INNER JOIN between customers and sales and apply a WHERE condition).`
```sql
SELECT 
COUNT(*) AS total_number
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
WHERE c.state = 'Delhi'
AND
s.order_status = 'Completed';
```
2. `Retrieve a list of products ordered by customers from the state 'Karnataka' with price greater than 10,000 (use INNER JOIN between sales, customers, and products).`
```sql
SELECT 
c.customer_name,
c.state,
p.product_name,
p.price
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
INNER JOIN products AS p
ON p.product_id = s.product_id
WHERE c.state = 'Karnataka'
AND
p.price >10000;
``` 
3. `List all customers who have placed orders where the product category is 'Accessories' and the order status is 'Completed' (use INNER JOIN with sales, customers, and products).`
```sql
SELECT 
c.customer_name
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
INNER JOIN products AS p
ON p.product_id = s.product_id
WHERE p.category = 'Accessories'
AND 
s.order_status = 'Completed';
```
4. `Show the order details of customers who have paid for their orders, excluding those who have cancelled their orders (use INNER JOIN between sales and payments and apply WHERE for order_status).`
```sql
SELECT 
s.*,
p.payment_status
FROM sales AS s
INNER JOIN payments AS p
ON s.order_id = p.order_id
WHERE 
s.order_status <> 'Cancelled';
```
5. `Retrieve products ordered by customers who are in the 'Gujarat' state and whose total order price is greater than 15,000 (use INNER JOIN between sales, customers, and products).`
```sql
SELECT 
p.product_name,
SUM(p.price) AS total_order_price
FROM products AS p
INNER JOIN sales AS s
ON p.product_id = s.product_id
INNER JOIN customers AS c
ON c.customer_id = s.customer_id
WHERE c.state = 'Gujarat'
GROUP BY p.product_name
HAVING SUM(p.price) > 150000;
```
6.'Find the total quantity of each product ordered by customers from 'Delhi' and only include products with a quantity greater than 5 (use INNER JOIN with sales, customers, and products and group by product).'
```sql
SELECT 
p.product_name,
COUNT(s.quantity) AS total_quantity
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id=s.customer_id
INNER JOIN products AS p
ON p.product_id = s.product_id
WHERE c.state = 'Delhi'
GROUP BY p.product_name
HAVING COUNT(s.quantity) > 5;
```
7. 'Get the average payment amount per customer who has placed more than 3 orders (use INNER JOIN between paymentsand sales, group by customer, and apply a HAVING clause).'
```sql
SELECT 
s.customer_id,
AVG(s.quantity*s.price_per_unit) AS avg_payment
FROM payments AS p
INNER JOIN sales AS s
ON p.order_id = s.order_id
GROUP BY s.customer_id,s.order_id
HAVING s.order_id > 3;
```
8.'Retrieve the total sales for each product category and only include categories where the total sales exceed 100,000 (use INNER JOIN between sales and products, group by category).'
```sql
SELECT 
p.category,
SUM(s.quantity*s.price_per_unit) AS total_sales
FROM products AS p
INNER JOIN sales AS s
ON p.product_id = s.product_id
GROUP BY p.category
HAVING COUNT(quantity*price_per_unit) > 100000;
```
9.'Show the number of customers in each state who have made purchases with a total spend greater than 50,000 (use INNER JOIN between sales and customers).'
```sql
SELECT 
c.state,
COUNT(DISTINCT c.customer_id) AS num_customers,
SUM(s.quantity*s.price_per_unit) AS total_sales
FROM sales AS s
INNER JOIN customers AS c
ON s.customer_id = c.customer_id
GROUP BY c.state
HAVING SUM(s.quantity*s.price_per_unit) >50000;
```
10.'List the total sales by brand for products that have been ordered more than 10 times (use INNER JOIN between salesand products, group by brand).'
```sql
SELECT 
p.brand, 
SUM(s.quantity * s.price_per_unit) AS total_sales
FROM sales s
INNER JOIN products p 
ON s.product_id = p.product_id
GROUP BY p.brand
HAVING COUNT(s.order_id) > 10;
```
11.'Retrieve the total sales per customer in 'Delhi' where the order status is 'Completed', only include those with total sales greater than 50,000, and order the results by total sales (use INNER JOIN between sales and customers).'
```sql
SELECT 
c.customer_name,
c.state,
SUM(s.quantity*s.price_per_unit) AS total_sales,
s.order_status
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
WHERE s.order_status = 'Completed'
AND
c.state = 'Delhi'
GROUP BY 1,2,4
HAVING SUM(s.quantity*s.price_per_unit) > 50000
ORDER BY total_sales ASC;
```
12.'Show the total quantity sold per product in the 'Accessories' category where the total quantity sold is greater than 50 and order the results by product name (use INNER JOIN between sales and products).'
```sql
SELECT
p.product_name,
SUM(s.quantity) AS total_quantity
FROM sales AS s
INNER JOIN products AS p
ON s.product_id = p.product_id
WHERE p.category = 'Accessories'
GROUP BY 1
HAVING SUM(s.quantity) > 50
ORDER BY p.product_name ASC;
```
13.'Find the total number of orders for customers from 'Maharashtra' who have spent more than 1,00,000, and order the results by the total amount spent (use INNER JOIN between sales and customers).'
```sql
SELECT 
c.customer_name,
COUNT(s.order_id) AS total_order,
SUM(s.quantity*s.price_per_unit) AS total_spent
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
WHERE c.state = 'Maharashtra'
GROUP BY 1
HAVING SUM(s.quantity*s.price_per_unit) > 100000
ORDER BY total_spent DESC;
```
14.'Get the number of orders per product and filter to include only products that have been ordered more than 10 times, then order the results by the highest number of orders (use INNER JOIN between sales and products).'
```sql
SELECT 
p.product_name,
COUNT(s.order_id) AS total_order
FROM sales AS s
INNER JOIN products AS p
ON s.product_id = p.product_id
GROUP BY p.product_id
HAVING COUNT(s.order_id) > 10
ORDER BY total_order DESC;
```
15.'Retrieve the number of payments made per customer where the payment status is 'Payment Successed' and group by customer, ordering by payment count (use INNER JOIN between payments and customers).'
```sql
SELECT 
c.customer_name,
COUNT(p.payment_id) AS payment_count
FROM customers AS c
INNER JOIN sales AS s
ON c.customer_id = s.customer_id
INNER JOIN payments AS p
ON p.order_id = s.order_id
WHERE p.payment_status = 'Payment Successed'
GROUP BY c.customer_name
ORDER BY payment_count;
```
---

## SQL Queries & Analysis

The `queries.sql` file contains all SQL queries developed for this project. Each query corresponds to a business problem and demonstrates skills in SQL syntax, data filtering, aggregation, grouping, and ordering.

---

## Getting Started

### Prerequisites
- PostgreSQL (or any SQL-compatible database)
- Basic understanding of SQL

### Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/flipkart-sql-project.git
   ```
2. **Set Up the Database**:
   - Run the `schema.sql` script to set up tables and insert sample data.

3. **Run Queries**:
   - Execute each query in `queries.sql` to explore and analyze the data.

---

## Questions & Feedback

Feel free to add your questions and code snippets below and submit them as issues for further feedback!

**Example Questions**:
1. **Question**: How can I filter orders placed in the last 6 months?
   **Code Snippet**:
   ```sql
   SELECT * FROM sales
   WHERE order_date >= CURRENT_DATE - INTERVAL '6 months';
   ```

---

## Contact Me

📄 **[Resume](#)**  
📧 **[Email](mailto:your.email@example.com)**  
📞 **Phone**: +123-456-7890  

---

## ERD (Entity-Relationship Diagram)

## Notice:
All customer names and data used in this project are computer-generated using AI and random
functions. They do not represent real data associated with Amazon or any other entity. This
project is solely for learning and educational purposes, and any resemblance to actual persons,
businesses, or events is purely coincidental.

![ERD Placeholder](https://github.com/najirh/Flipkart--SQL-Project-B01/blob/main/Flipkart%20Project%20Schemas.png)
