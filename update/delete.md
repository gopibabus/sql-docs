# 🔥Deleting Data

## ⚡Deleting Rows

```sql
DELETE FROM invoices
WHERE invoice_id = 1;
```

```sql
DELETE FROM invoices
WHERE client_id = 
			(SELECT client_id
			FROM clients
			WHERE name = 'Myworks'
			);
```