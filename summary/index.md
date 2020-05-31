# 🔥Summarizing Data

## ⚡Aggregate Functions

 <img alt="functions" width="150px" src="/assets/images/functions.png" />

?> An **aggregate function** allows you to perform a calculation on a set of values to return a single scalar value. We often use aggregate functions with the **GROUP BY** and **HAVING** clauses of the **SELECT** statement.

* MAX()
* MIN()
* AVG()
* SUM()
* COUNT()

```sql
SELECT 
	MAX(invoice_total) AS highest,
	MIN(invoice_total) AS lowest,
	AVG(invoice_total) AS average,
	SUM(invoice_total) AS total,
	COUNT(*) AS total_records
FROM invoices;
```

```sql
SELECT 
	MAX(invoice_total) AS highest,
	MIN(invoice_total) AS lowest,
	AVG(invoice_total) AS average,
	SUM(invoice_total) AS total,
	COUNT(DISTINCT client_id) AS total_records
FROM invoices
WHERE invoice_date > '2019-07-01';
```

```sql
SELECT 
	'First half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date
	BETWEEN '2019-01-01' AND '2019-06-30'
UNION
SELECT 
	'Second half of 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date
	BETWEEN '2019-07-01' AND '2019-12-30'
UNION
SELECT 
	'Total in 2019' AS date_range,
    SUM(invoice_total) AS total_sales,
    SUM(payment_total) AS total_payments,
    SUM(invoice_total - payment_total) AS what_we_expect
FROM invoices
WHERE invoice_date
	BETWEEN '2019-01-01' AND '2019-12-30'
```

## ⚡GROUP BY clause

> The **GROUP BY** statement groups rows that have the same values into summary rows

?> Here just observe **order of execution** of different clauses in a query.

```sql
SELECT
	client_id,
    SUM(invoice_total) AS total_sales
FROM invoices
WHERE invoice_date >= '2019-07-01'
GROUP BY client_id
ORDER BY total_sales DESC;
```

```sql
SELECT
	state,
    city,
    SUM(invoice_total) AS total_sales
FROM invoices i
JOIN clients USING (client_id)
GROUP BY client_id;
```

```sql
SELECT
	date,
    pm.name AS payment_method,
    SUM(amount) AS total_payments
FROM payments p
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
GROUP BY date, payment_method
ORDER BY date;
```

## ⚡HAVING clause

?> The **HAVING** clause was added to SQL because the **WHERE** keyword could not be used with aggregate functions.

?> **HAVING** is used after **GROUP BY** and **WHERE** is used before **GROUP BY**.

?> columns that we use in **HAVING** clause should be part of **SELECT** clause in the query.

```sql
SELECT
	client_id,
    SUM(invoice_total) AS total_sales,
    COUNT(*) AS number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 500 AND number_of_invoices > 5;
```

> Following example is similar to most real world problems

```sql
SELECT 
	c.customer_id,
    c.first_name,
    c.last_name,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM customers c
JOIN orders o USING(customer_id)
JOIN order_items oi USING (order_id)
WHERE state = 'VA'
GROUP BY 
	c.customer_id,
    c.first_name,
    c.last_name
HAVING total_sales > 100;
```

## ⚡ROLLUP Operator

?> The **ROLLUP** is an extension of the **GROUP BY** clause. The ROLLUP option allows you to include extra rows that represent the subtotals, which are commonly referred to as super-aggregate rows, along with the grand total row. By using the ROLLUP option, you can use a single query to generate multiple grouping sets.

```sql
SELECT
	state,
    city,
    SUM(invoice_total) AS total_sales
FROM invoices i
JOIN clients c USING (client_id)
GROUP BY state, city WITH ROLLUP;
```

```sql
SELECT 
	pm.name AS payment_method,
    SUM(amount) AS total
FROM payments p
JOIN payment_methods pm
	ON p.payment_method = pm.payment_method_id
GROUP BY pm.name WITH ROLLUP;
```