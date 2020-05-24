# 🔥Inserting Data

## ⚡Inserting a Row

?> We can pass **DEFAULT** as value to the query, so that mysql will insert default value that we defined in our schema.

```sql
INSERT INTO customers
VALUES(
	DEFAULT, 
    'John', 
    'Smith', 
    '1990-01-01', 
    NULL, 
    '192 Carlton Avenue', 
    'Jersey City', 
    'NJ', 
    DEFAULT
);

-- Alternative

INSERT INTO customers (
	first_name, 
    last_name, 
    birth_date, 
    address, 
    city, 
    state
)
VALUES(
    'John', 
    'Smith', 
    '1990-01-01', 
    '192 Carlton Avenue', 
    'Jersey City', 
    'NJ'
);
```

## ⚡Inserting multiple Rows

```sql
INSERT INTO shippers (name)
VALUES ('Shipper 1'),
	   ('Shipper 2'),
       ('Shipper 3');
```

```sql
INSERT INTO products (name, quantity_in_stock, unit_price)
VALUES ('Product1', 10, 1.95),
	   ('Product2', 11, 2.95),
       ('Product3', 12, 3.95);
```

## ⚡Inserting Hierarchical Rows

> In the following example **order** table is parent of **order_items** table.

```sql
INSERT INTO orders (customer_id, order_date, status)
VALUES ('1', '2019-01-02', 1);

INSERT INTO order_items
VALUES 
	(LAST_INSERT_ID(), 1, 1, 2.95),
	(LAST_INSERT_ID(), 2, 1, 3.95);
```

## ⚡Creating a Copy of a Table

```sql
CREATE TABLE orders_archived AS 
SELECT * FROM orders;

TRUNCATE orders_archived;
```

```sql
INSERT INTO orders_archived
SELECT *
FROM orders 
WHERE order_date < '2019-01-01';
```

```sql
CREATE TABLE invoices_archived AS
SELECT 
	i.invoice_id,
    i.number,
    c.name AS client,
    i.invoice_total,
    i.payment_total,
    i.invoice_date,
    i.payment_date,
    i.due_date
FROM invoices i
JOIN clients c
	USING (client_id)
WHERE payment_date IS NOT NULL;
```