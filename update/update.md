# 🔥Updating Data

## ⚡Updating a Single Row

```sql
UPDATE invoices
SET payment_total = 10, payment_date = '2019-03-01'
WHERE invoice_id = 1;
```

```sql
UPDATE invoices
SET
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE invoice_id = 3;
```

## ⚡Updating a Multiple Rows

```sql
UPDATE invoices
SET
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE client_id IN (3, 4);
```

```sql
UPDATE customers
SET points = points + 50
WHERE birth_date < '1990-01-01';
```

## ⚡Using Sinqueries in updates

```sql
UPDATE invoices
SET
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE client_id =
				(SELECT client_id
                FROM clients
                WHERE name = 'Myworks');
```

```sql
UPDATE invoices
SET
	payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE client_id IN
				(SELECT client_id
                FROM clients
                WHERE state IN ('CA', 'NY'));
```

```sql
UPDATE orders
SET
	comments = 'Gold customer'
WHERE customer_id IN
				(SELECT customer_id
                FROM customers
                WHERE points > 3000);
```
