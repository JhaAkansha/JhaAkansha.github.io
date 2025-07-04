---
title: "Understanding SQL Joins"
date: 2023-07-02T22:23:59+05:30
---

# Understanding SQL Joins: A Quick Guide

When working with relational databases, **SQL joins** allow you to combine rows from two or more tables based on a related column. Here's a quick breakdown of the most common types of joins:

---

## INNER JOIN

**Returns**: Only rows that have matching values in both tables.

```sql
SELECT *
FROM orders
INNER JOIN customers ON orders.customer_id = customers.id;
```
Use it when you want to retrieve only the data that exists in both databases.

## LEFT JOIN (or LEFT OUTER JOIN)

**Returns**: All rows from the left table, and the matched rows from the right table. If no match, returns `NULL` on the right side.

```sql
SELECT *
FROM customers
LEFT JOIN orders ON customer.id = orders.customer_id;
```
Use it when you want all records from the left table, regardless of matches.

## RIGHT JOIN (or RIGHT OUTER JOIN)
**Returns**: All rows from the right table, and the matched rows from the left table. If no match, returns `NULL` on the left side.

```sql
SELECT *
FROM orders
RIGHT JOIN customers ON orders.customer_id = customers.id;
```
Use it when you want all records from the right table, with or without matches.

## FULL JOIN (or FULL OUTER JOIN)
**Returns**: All rows when there is a match either left or right table. Non-matching rows will have `NULL` where appropriate.

```sql
SELECT *
FROM customers
FULL OUTER JOIN orders ON  customers.id = orders.customer_id;
```
Use it when you want to see everything from both tables, matched or not.

## CROSS JOIN
**Returns**: The Cartesian product of the two tables (every combination of rows).

```sql
SELECT *
FROM products
CROSS JOIN categories;
```
Use with caution - can return a very large number of rows.

## SELF JOIN
**Returns**: A join of a table to itself, useful for hierarchical or related data within one table.

```sql
SELECT A.name AS Employee, B.name AS Manager
FROM employees A
JOIN employees B ON A.manager_id = B.id;
```
Use it when you need to compare rows within the same table.