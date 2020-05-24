# 🔥Retrieving Data from Single Table

<img alt="table" width="300px" src="/assets/images/table.png" />

## ⚡SELECT
### ✳Retrieve entire data from customers table

```sql
SELECT * FROM customers;
```

### ✳Retrieve specific column data from customers table

```sql
SELECT first_name, last_name FROM customers;
```

### ✳Use Mathematical Expressions in SQL queries

```sql
SELECT first_name, last_name, points, (points + 10) * 100 FROM customers;
```

### ✳Use alias in SQL queries

```sql
SELECT first_name, last_name, points, (points + 10) * 100 AS discount_factor FROM customers;

SELECT first_name, last_name, points, (points + 10) * 100 AS 'discount factor' FROM customers;
```

### ✳Retrieve unique set of state data from customers table

```sql
SELECT DISTINCT state FROM customers;
```

## ⚡WHERE

### ✳Retrieve filtered data from customers table

```sql
SELECT * FROM customers WHERE points > 3000;

SELECT * FROM customers WHERE state = 'VA';

SELECT * FROM customers WHERE state != 'VA';
-- Alternative
SELECT * FROM customers WHERE state <> 'VA';

SELECT * FROM customers WHERE birth_date > '1990-01-01';
SELECT * FROM orders WHERE order_date >= '2019-01-01';
```

## ⚡AND, OR and NOT
### ✳Combining multiple filters

!> Please consider precedence of operators **(AND > OR)**

```sql
SELECT * 
FROM customers
WHERE
	birth_date >= '1990-01-01'
  AND
  points > 1000;
```

```sql
SELECT * 
FROM customers 
WHERE 
	birth_date >= '1990-01-01' 
  OR
  points > 1000;
```

```sql
SELECT * 
FROM customers 
WHERE 
	birth_date >= '1990-01-01' 
  OR 
  (points > 1000 AND state = 'VA');
```

```sql
SELECT * 
FROM customers 
WHERE NOT (birth_date >= '1990-01-01' OR points > 1000);
```

```sql
SELECT * 
FROM order_items 
WHERE order_id = 6 AND (unit_price * quantity) > 30;
```

## ⚡IN
### ✳Find match in the given arguments

```sql
SELECT * 
FROM customers 
WHERE state = 'VA' OR state = 'GA' OR state = 'FL';

-- Alternative

SELECT * 
FROM customers 
WHERE state IN ('VA', 'GA', 'FL');
```

```sql
SELECT * 
FROM customers 
WHERE state NOT IN ('VA', 'GA', 'FL');
```

```sql
SELECT * 
FROM products 
WHERE quantity_in_stock IN (49, 38, 72);
```

## ⚡BETWEEN
### ✳Retrieve values within a range

```sql
SELECT * 
FROM customers 
WHERE points >= 1000 AND points <= 3000;

-- Alternative

SELECT * 
FROM customers 
WHERE points BETWEEN 1000 AND 3000;
```

```sql
SELECT * 
FROM customers 
WHERE birth_date BETWEEN '1990-01-01' AND '2000-01-01';
```

## ⚡LIKE
### ✳Matches a specified pattern

```sql
-- Customers last name starting with "b"(any no of chars after b)
SELECT * 
FROM customers 
WHERE last_name LIKE 'b%';
```

```sql
-- Customers last name contains any number of characters before and after "b"
SELECT * 
FROM customers 
WHERE last_name LIKE '%b%';
```

```sql
-- Customers last name ends with "y"
SELECT * 
FROM customers 
WHERE last_name LIKE '%y';
```

```sql
-- Customers last name ends with "y" and having exactly 5 chars infront of "y"
SELECT * 
FROM customers 
WHERE last_name LIKE '_____y';
```

```sql
-- Customers last name starts with "b" ends with "y" and having exactly 4 chars in between
SELECT * 
FROM customers 
WHERE last_name LIKE 'R____y';
```

```sql
-- Customers address containing "avenue"
SELECT * 
FROM customers 
WHERE address LIKE '%trail%' OR address LIKE '%avenue%';
```

## ⚡REGEXP
### ✳Matches a specified pattern

```sql
-- Customers last_name containing "field"
SELECT * 
FROM customers 
WHERE last_name LIKE '%field%';

--Alternative

SELECT * 
FROM customers 
WHERE last_name REGEXP 'field';
```

```sql
-- Customers last_name start with "field"
SELECT * 
FROM customers 
WHERE last_name REGEXP '^field';
```

```sql
-- Customers last_name end with "field"
SELECT * 
FROM customers 
WHERE last_name REGEXP 'field$';
```

```sql
-- Customers last_name contain "field" or "mac"
SELECT * 
FROM customers 
WHERE last_name REGEXP 'field|mac';
```

```sql
-- Customers last_name start with "field" or "mac"
SELECT * 
FROM customers 
WHERE last_name REGEXP '^field|mac';
```

```js
-- Customers last_name contain "ge" or "ie" or "me"
SELECT * 
FROM customers 
WHERE last_name REGEXP '[gim]e';
```

```sql
-- Customers last_name starts with a to h preceding by "e"
SELECT * 
FROM customers 
WHERE last_name REGEXP '[a-h]e';
```

```sql
-- Get Customers whose first names are ELKA or AMBUR
SELECT * 
FROM customers 
WHERE first_name REGEXP 'elka|ambur';
```

```sql
-- Get Customers whose last names end with EY or ON
SELECT * 
FROM customers 
WHERE last_name REGEXP 'EY$|ON$';
```

```sql
-- Get Customers whose last names start with MY or contains SE
SELECT * 
FROM customers 
WHERE last_name REGEXP '^my|se';
```

```sql
-- Get Customers whose last names contain B followed by R or U
SELECT * 
FROM customers 
WHERE last_name REGEXP 'b[ru]';
```

## ⚡NULL
### ✳Missing values

```sql
-- Get Customers who are not having phones
SELECT * 
FROM customers 
WHERE phone IS NULL;
```

```sql
-- Get Orders those are not shipped
SELECT * 
FROM orders 
WHERE shipped_date IS NULL;
```

## ⚡ORDER BY
### ✳Sort the result-set in Ascending or Descending order

> **💡NOTE:** By default contents in a table is sorted by the primary key.

```sql
-- Get Customers whose first names sorted in Ascending Order
SELECT * 
FROM customers 
ORDER BY first_name;

--- Descending

-- Get Customers whose first names sorted in Descending Order
SELECT * 
FROM customers 
ORDER BY first_name DESC;
```

```sql
-- Get Customers by state in Descending order and first name in Descending Order
SELECT * 
FROM customers 
ORDER BY state DESC, first_name DESC;
```

```sql
-- Get Customers sorted by first 2 columns [This is not a ideal way of sorting]
SELECT * 
FROM customers 
ORDER BY 1, 2;
```

```sql
-- Get order items sorted by total price of available units
SELECT *, quantity * unit_price AS total_price
FROM order_items 
ORDER BY total_price DESC;
```

## ⚡LIMIT
### ✳Specify the number of records to return

!> **NOTE:** LIMIT clause always come at end of the query.

```sql
-- Get first 3 customers
SELECT *
FROM customers LIMIT 3;
```

```sql
-- page 1 : 1 - 3
-- page 1 : 4 - 6
-- page 1 : 7 - 9
-- Get 3 customers by skipping first 6 customers (Pagination)
SELECT *
FROM customers LIMIT 6, 3;
-- Here 6 is offset
```

```sql
-- Get top 3 loyal customers
SELECT *
FROM customers 
ORDER BY points DESC
LIMIT 3;
```

## ⚡Order of Execution

?> The SQL **order of execution** defines the order in which the clauses of a query are evaluated.

<img alt="order of clauses" width="700px" src="/assets/images/order_clauses.png" />

<img alt="order of clauses" width="700px" src="/assets/images/order_clauses2.png" />