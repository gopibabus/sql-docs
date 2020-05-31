# 🔥Complex Queries

 <img alt="complex queries" width="500px" src="/assets/images/complex_sql.png" />

### ✳Subqueries

```sql
-- Find products that are more
-- expensive than lettuce (product_id = 3)

SELECT *
FROM products
WHERE unit_price > (
	SELECT unit_price
    FROM products
    WHERE product_id = 3
);
```

```sql
-- In sql_hr database:
-- Find employees who earn more than average.

SELECT *
FROM employees
WHERE salary > (
	SELECT AVG(salary)
    FROM employees
);
```

### ✳The IN Operator

```sql
-- Find the products that have never been ordered

SELECT *
FROM products
WHERE product_id NOT IN (
	SELECT DISTINCT product_id
    FROM order_items
);
```

```sql
-- Find clients without invoices

SELECT *
FROM clients
WHERE client_id NOT IN (
	SELECT DISTINCT client_id
	FROM invoices
);
```

### ✳Subqueries vs Joins

```sql
-- Find clients without invoices

SELECT *
FROM clients
WHERE client_id NOT IN (
	SELECT DISTINCT client_id
	FROM invoices
);

-- Alternative

SELECT *
FROM clients
LEFT JOIN invoices USING (client_id)
WHERE invoice_id IS NULL;
```

```sql
-- Find customers who have ordered lettuce (id=3)
-- select customer_id, first_name, last_name

SELECT DISTINCT customer_id, first_name, last_name
FROM customers
WHERE customer_id IN (
	SELECT o.customer_id
    FROM order_items oi
    JOIN orders o USING (order_id)
    WHERE product_id = 3
);

-- Alternative

SELECT DISTINCT customer_id, first_name, last_name
FROM customers c
JOIN orders o USING(customer_id)
JOIN order_items oi USING (order_id)
WHERE oi.product_id = 3;
```

### ✳The ALL keyword

```sql
-- Select invoices larger than all invoices of
-- client 3

SELECT *
FROM invoices
WHERE invoice_total > (
	SELECT MAX(invoice_total)
    FROM invoices
    WHERE client_id = 3
);

-- Alternative

SELECT *
FROM invoices
WHERE invoice_total > ALL(
	SELECT invoice_total
    FROM invoices
    WHERE client_id = 3
);
```

### ✳The ANY keyword

```sql
-- Select clients with at least 2 invoices

SELECT *
FROM clients
WHERE client_id IN (
	SELECT client_id
    FROM invoices
    GROUP BY client_id
    HAVING COUNT(*) >=2
);

-- Alternative

SELECT *
FROM clients
WHERE client_id = ANY (
	SELECT client_id
    FROM invoices
    GROUP BY client_id
    HAVING COUNT(*) >=2
);
```

### ✳Correlated Subqueries

```sql
-- for each employee
-- 		calculate the avg salary for employee.office
--		return the employee if salary > avg

SELECT *
FROM employees e
WHERE salary > (
	SELECT AVG(salary)
    FROM employees
    WHERE office_id = e.office_id
);
```

```sql
-- Get invoices that are larger than the
-- client's average invoice amount

SELECT *
FROM invoices i
WHERE invoice_total > (
	SELECT AVG(invoice_total)
    FROM invoices
    WHERE client_id = i.client_id
);
```

### ✳The EXISTS Operator

```sql
-- select clients that have an invoice

SELECT *
FROM clients
WHERE client_id IN (
	SELECT DISTINCT client_id
    FROM invoices
);

-- Alternative

SELECT *
FROM clients c
WHERE EXISTS (
	SELECT client_id
    FROM invoices
    WHERE client_id = c.client_id
);
```

```sql
-- Find products that have never been ordered

SELECT *
FROM products
WHERE product_id NOT IN (
	SELECT product_id
    FROM order_items
);

-- Alternative

SELECT *
FROM products p
WHERE NOT EXISTS (
	SELECT product_id
    FROM order_items
    WHERE product_id = p.product_id
);
```

### ✳Subqueries in the SELECT Clause

```sql
SELECT
	invoice_id,
    invoice_total,
    (SELECT AVG(invoice_total)
		FROM invoices) AS invoice_average,
	invoice_total - (SELECT invoice_average) AS difference
FROM invoices;
```

```sql
SELECT
	client_id
    name,
    (SELECT SUM(invoice_total)
		FROM invoices
        WHERE client_id = c.client_id) AS total_sales,
	(SELECT AVG(invoice_total) FROM invoices) AS average,
    (SELECT total_sales - average) AS difference
FROM clients c;
```

### ✳Subqueries in FROM Caluse

```sql
SELECT *
FROM (
	SELECT
	client_id
    name,
    (SELECT SUM(invoice_total)
		FROM invoices
        WHERE client_id = c.client_id) AS total_sales,
	(SELECT AVG(invoice_total) FROM invoices) AS average,
    (SELECT total_sales - average) AS difference
FROM clients c
) AS sales_summary
WHERE total_sales IS NOT NULL;
```
