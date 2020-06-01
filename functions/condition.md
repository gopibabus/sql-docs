## ⚡Conditional Statements

### ✳IFNULL()

?> The **IFNULL()** function returns a specified value if the expression is NULL. If the expression is NOT NULL, this function returns the expression.

```sql
SELECT
	order_id,
    IFNULL(shipper_id, 'Not assigned') AS shipper
FROM orders;
```

### ✳COALESCE()

?> The **COALESCE()** function returns the first non-null value in a list.

```sql
SELECT
	order_id,
    COALESCE(shipper_id, comments, 'Not assigned') AS shipper
FROM orders;
```

```sql
SELECT
	CONCAT(first_name, ' ', last_name) AS customer,
    COALESCE(phone, 'Unknown') AS phone
FROM customers;
```

### ✳IF()

?> he **IF()** function returns a value if a condition is TRUE, or another value if a condition is FALSE.

```sql
SELECT
	order_id,
    order_date,
    IF(
		YEAR(order_date) = YEAR(NOW()),
        'Active',
        'Archived'
    ) AS category
FROM orders;
```

### ✳CASE Statement

?> The **CASE** statement goes through conditions and returns a value when the first condition is met (like an IF-THEN-ELSE statement). So, once a condition is true, it will stop reading and return the result. If no conditions are true, it returns the value in the ELSE clause. If there is no ELSE part and no conditions are true, it returns NULL.

```sql
SELECT
	order_id,
    order_date,
    CASE
		WHEN YEAR(order_date) = YEAR(NOW()) THEN 'Active'
		WHEN YEAR(order_date) = YEAR(NOW()) - 1 THEN 'Last Year'
		WHEN YEAR(order_date) < YEAR(NOW()) - 1 THEN 'Archived'
        ELSE 'Future'
	END AS category
FROM orders;
```
