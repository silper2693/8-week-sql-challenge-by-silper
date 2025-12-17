# 🍜 Case Study #1: Danny's Diner

<p align="center"><img width="413" height="413" alt="image" src="https://github.com/user-attachments/assets/d9ddbe42-190d-4c2c-8c3e-9cc63c4e9104" /></p>

<p align="center">Introduction</p>

<p align="justify">Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favourite foods: sushi, curry and ramen.</p>

<p align="justify">Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.</p>

## 📌 Table of Contents
- [💡 Business Talk](#-business-talk)
- [🔗 Entity Relationship Diagram](#-entity-relationship-diagram)

To find out more: [here](https://8weeksqlchallenge.com/case-study-1/)

## 💡 Business Talk
Danny wants to use sales data to understand customer spending behavior, identify popular menu items, and evaluate the impact of the membership program on revenue and customer loyalty.

## 🔗 Entity Relationship Diagram

<p align="center"><img width="706" height="357" alt="image" src="https://github.com/user-attachments/assets/72565f79-fe6d-43f2-8a99-440dbb5c9cf8" /></p>

## 🧠 Question & Solution
### 1. What is the total amount each customer spent at the restaurant?
```sql
SELECT
  customer_id
  ,SUM(price) AS total_spent
FROM dannys_diner.sales AS s
INNER JOIN menu AS m
  ON s.product_id = m.product_id
GROUP BY customer_id
ORDER BY total_spent DESC;
```

### 2. How many days has each customer visited the restaurant?
```sql
SELECT
  customer_id
  ,COUNT(DISTINCT order_date) AS quantity_visitor
FROM sales
GROUP BY customer_id;
```
### 3. What was the first item from the menu purchased by each customer?
```sql
WITH base_orders AS (
  SELECT
    s.customer_id
    ,s.order_date
    ,m.product_name
    ,RANK() OVER (
      PARTITION BY s.customer_id
      ORDER BY s.order_date) AS order_rank
    ,ROW_NUMBER() OVER (
      PARTITION BY s.customer_id
      ORDER BY s.order_date) AS order_row_num
  FROM sales AS s
  INNER JOIN menu AS m
    ON s.product_id = m.product_id
)

SELECT
  customer_id
  ,order_date
  ,product_name
FROM base_orders
WHERE order_rank = 1
  AND order_row_num = 1;
```




















