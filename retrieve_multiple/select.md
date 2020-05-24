# 🔥Retrieving Data from Multiple Tables

<img alt="tables" width="400px" src="/assets/images/tables_multiple.png" />

## ⚡Inner Joins
### ✳Matching values in both tables

```sql
-- Get all orders Where orders customer id equal to customers customer id
SELECT *
FROM orders
JOIN customers 
	ON orders.customer_id = customers.customer_id;
```

```sql
-- Get order_id, customer_id, first_name orders Where orders customer id equal to customers customer id
SELECT order_id, o.customer_id, first_name
FROM orders o
JOIN customers c 
	ON o.customer_id = c.customer_id;
```

```sql
-- Get order_items Where orders product id equal to products product id
SELECT order_id, oi.product_id, quantity, oi.unit_price
FROM order_items oi
JOIN products p 
	ON oi.product_id = p.product_id;
```

### ✳Matching values in tables across databases

```sql
USE sql_inventory;

SELECT *
FROM sql_store.order_items oi
JOIN products p
	ON oi.product_id = p.product_id;
```

## ⚡Self Joins
### ✳Table is joined with itself

```sql
USE sql_hr;

SELECT 
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
JOIN employees m
	ON e.reports_to = m.employee_id;
```

## ⚡Joining Multiple Tables

```sql
USE sql_store;

SELECT 
	o.order_id,
    o.order_date,
    c.first_name,
    c.last_name,
    os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
JOIN order_statuses os
	ON o.status = os.order_status_id;
```

```sql
USE sql_invoicing;

SELECT 
	p.date,
    p.invoice_id,
    p.amount,
    c.name,
    pm.name
FROM payments p
JOIN clients c
	ON p.client_id = c.client_id
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id;
```

## ⚡Compound Join Conditions

?> **Composite primary key**: Table with 2 primary keys to select unique data.

```sql
SELECT * 
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_Id = oin.order_Id
    AND oi.product_id = oin.product_id;`
```

## ⚡Implicit Join Syntax

?> It is good practice to use **JOIN** explicitly while joining multiple tables.

```sql
SELECT * 
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id;

-- Implicit Join Syntax
SELECT * 
FROM orders o, customers c
	WHERE o.customer_id = c.customer_id;
```

## ⚡Outer Joins
### ✳LEFT JOIN
?> The **LEFT JOIN** keyword returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side, if there is no match.

```sql
SELECT 
	c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
	ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

### ✳RIGHT JOIN
?> The **RIGHT JOIN** keyword returns all records from the right table (table2), and the matched records from the left table (table1). The result is NULL from the left side, when there is no match.

```sql
SELECT 
	c.customer_id,
    c.first_name,
    o.order_id
FROM orders o
RIGHT JOIN customers c
	ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

```sql
SELECT 
	p.product_id,
    p.name,
    oi.quantity
FROM products p
LEFT JOIN order_items oi
	ON p.product_id = oi.product_id;
```

## ⚡Outer Join between multiple Tables

!> **Alert:** Please don't mix RIGHT JOIN with LEFT JOINS , The best practice is to use only one type of Outer join in a sql query

```sql
SELECT 
	c.customer_id,
    c.first_name,
    o.order_id,
    sh.name shipper
FROM customers o
LEFT JOIN orders o
	ON c.customer_id = o.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
ORDER BY c.customer_id;
```

```sql
SELECT 
	o.order_id,
    o.order_date,
    c.first_name AS customer,
    sh.name AS shipper,
    os.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
LEFT JOIN shippers sh
	ON o.shipper_id = sh.shipper_id
JOIN order_statuses os
	ON o.status = os.order_status_id;
```

## ⚡Self Outer Joins

```sql
SELECT 
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m
	ON e.reports_to = m.employee_id;
```

## ⚡USING clause

?> **USING** Clause is used to match only one column when more than one column matches.

```sql
SELECT 
    c.first_name,
    o.order_id
FROM orders o
JOIN customers c
	ON c.customer_id = o.customer_id;

-- Alternative

SELECT 
    c.first_name,
    o.order_id
FROM orders o
JOIN customers c
	USING (customer_id);
```

```sql
SELECT 
    c.first_name,
    o.order_id,
    sh.name AS shipper
FROM orders o
JOIN customers c
	USING (customer_id)
LEFT JOIN shippers sh
	USING (shipper_id);
```

```sql
SELECT *
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_Id AND
    oi.product_id = oin.product_id;

-- Alternative

SELECT *
FROM order_items oi
JOIN order_item_notes oin
	USING(order_Id, product_id);
```

```sql
SELECT 
	p.date,
    c.name AS client,
    p.amount,
    pm.name AS payment_method
FROM payments p
JOIN clients c USING (client_id)
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id;
```

## ⚡Natural Joins

?> Columns with the same name of associated tables will appear once only.

!> **Alert:** These are plain and simple and you are giving permission to DBMS to select columns. It will result in returning un expected results. So better not to use them.

```sql
SELECT 
    c.first_name,
    o.order_id
FROM orders o
JOIN customers c
	ON c.customer_id = o.customer_id;

-- Alternative

SELECT 
	o.order_id,
    c.first_name
FROM orders o
NATURAL JOIN customers c;
```

## ⚡Cross Joins

?> The SQL **CROSS JOIN** produces a result set which is the number of rows in the first table multiplied by the number of rows in the second table if no WHERE clause is used along with CROSS JOIN.This kind of result is called as Cartesian Product. If WHERE clause is used with CROSS JOIN, it functions like an INNER JOIN.

```sql
SELECT 
    c.first_name AS customer,
    p.name AS product
FROM customers c
CROSS JOIN products p
ORDER BY c.first_name;
```

## ⚡Unions

?> The **UNION** operator is used to combine the result-set of two or more SELECT statements.

* Each SELECT statement within UNION must have the same number of columns.
* The columns must also have similar data types.
* The columns in each SELECT statement must also be in the same order.

```sql
SELECT
order_id,
order_date,
'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01'
-- end of 1st selection
UNION
-- start of 2nd selection
SELECT
order_id,
order_date,
'Archived' AS status
FROM orders
WHERE order_date < '2019-01-01';
```