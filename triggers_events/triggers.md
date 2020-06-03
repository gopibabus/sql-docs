# 🔥Triggers

<img alt="triggers" width="500px" src="/assets/images/triggers.png" />

?>**Trigger** is a block of SQL code that automatically gets executed before or after an insert,update or delete statement.

```sql
DELIMITER $$

CREATE TRIGGER payments_after_insert
	AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;
END $$
DELIMITER ;

-- In order to check for our trigger

SELECT * FROM sql_invoicing.invoices;
INSERT INTO payments VALUES (DEFAULT, 5, 3, '2019-01-01', 10, 1);
```

```sql
DELIMITER $$

CREATE TRIGGER payments_after_delete
	AFTER DELETE ON payments
    FOR EACH ROW
BEGIN
	UPDATE invoices
    SET payment_total = payment_total - OLD.amount
    WHERE invoice_id = OLD.invoice_id;
END $$
DELIMITER ;

--- In order to check trigger

SELECT * FROM sql_invoicing.invoices;
DELETE FROM payments WHERE payment_id = 10;
```

### ✳Viewing Triggers

```sql
SHOW TRIGGERS;

SHOW TRIGGERS LIKE 'payments%';
```

### ✳Dropping Triggers

```sql
DROP TRIGGER IF EXISTS payments_after_insert;
```

### ✳Using Triggers for Auditing

```sql
DROP TRIGGER IF EXISTS payments_after_insert;

DELIMITER $$
CREATE TRIGGER payments_after_insert
    AFTER INSERT ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices
    SET payment_total = payment_total + NEW.amount
    WHERE invoice_id = NEW.invoice_id;

    INSERT INTO payments_audit
    VALUES(NEW.client_id, NEW.date, NEW.amount, 'Insert', NOW());
END $$
DELIMITER ;

-- In order to check for our trigger

SELECT * FROM sql_invoicing.invoices;
INSERT INTO payments VALUES (DEFAULT, 5, 3, '2019-01-01', 10, 1);

```

```sql
DROP TRIGGER IF EXISTS payments_after_delete;

DELIMITER $$
CREATE TRIGGER payments_after_delete
    AFTER DELETE ON payments
    FOR EACH ROW
BEGIN
    UPDATE invoices
    SET payment_total = payment_total - OLD.amount
    WHERE invoice_id = OLD.invoice_id;

    INSERT INTO payments_audit
    VALUES(OLD.client_id, OLD.date, OLD.amount, 'Delete', NOW());
END $$
DELIMITER ;

-- In order to check trigger

SELECT * FROM sql_invoicing.invoices;
DELETE FROM payments WHERE payment_id = 10;
```
