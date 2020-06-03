# 🔥Parameters

### ✳Parameters

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	SELECT * FROM clients c
    WHERE c.state = state;
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
```

```sql
DROP PROCEDURE IF EXISTS get_invoices_by_client;

DELIMITER $$
CREATE PROCEDURE get_invoices_by_client(client_id INT)
BEGIN
	SELECT * FROM invoices i
    WHERE i.client_id = client_id;
END$$
DELIMITER ;

CALL get_invoices_by_client(1);
```

### ✳Parameters with default values

?> Version 1

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	IF state IS NULL THEN
		SET state = 'CA';
	END IF;

	SELECT * FROM clients c
    WHERE c.state = state;
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
CALL get_clients_by_state(NULL);
```

?> Version 2

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	IF state IS NULL THEN
		SELECT * FROM clients;
	ELSE
		SELECT * FROM clients c
		WHERE c.state = state;
	END IF;
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
CALL get_clients_by_state(NULL);
```

?> Version3

```sql
DROP PROCEDURE IF EXISTS get_clients_by_state;

DELIMITER $$
CREATE PROCEDURE get_clients_by_state(state CHAR(2))
BEGIN
	SELECT * FROM clients c
	WHERE c.state = IFNULL(state, c.state);
END$$
DELIMITER ;

CALL get_clients_by_state('CA');
CALL get_clients_by_state(NULL);
```

```sql
DROP PROCEDURE IF EXISTS get_payments;

DELIMITER $$
CREATE PROCEDURE get_payments(client_id INT, payment_method_id TINYINT)
BEGIN
	SELECT * FROM payments p
	WHERE
		p.client_id = IFNULL(client_id, p.client_id)
		AND
		p.payment_method = IFNULL(payment_method_id, p.payment_method);
END$$
DELIMITER ;

CALL get_payments(5, 2);
CALL get_payments(NULL, NULL);
```

### ✳Parameter Validation

?> Before Validation

```sql
DROP PROCEDURE IF EXISTS make_payment;

DELIMITER $$
CREATE PROCEDURE make_payment
	(
		invoice_id INT,
        payment_amount DECIMAL(9, 2),
        payment_date DATE
	)
BEGIN
	UPDATE invoices i
	SET
		i.payment_total = payment_amount,
		i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
END$$
DELIMITER ;

CALL make_payment(2, 100, '2019-01-01');
```

?> After adding Validation

```sql
DROP PROCEDURE IF EXISTS make_payment;

DELIMITER $$
CREATE PROCEDURE make_payment
	(
		invoice_id INT,
        payment_amount DECIMAL(9, 2),
        payment_date DATE
	)
BEGIN
	IF payment_amount <= 0 THEN
		SIGNAL SQLSTATE '22003'
			SET MESSAGE_TEXT = 'Invalid payment amount';
	END IF;

	UPDATE invoices i
	SET
		i.payment_total = payment_amount,
		i.payment_date = payment_date
	WHERE i.invoice_id = invoice_id;
END$$
DELIMITER ;

CALL make_payment(2, -100, '2019-01-01');#Throws Error code 22003
```

> [🌐 SQLSTATE common error codes](https://www.ibm.com/support/knowledgecenter/en/SSEPEK_11.0.0/codes/src/tpc/db2z_sqlstatevalues.html)

### ✳Output Parameters

```sql
DROP PROCEDURE IF EXISTS get_unpaid_invoices_for_client;

DELIMITER $$
CREATE PROCEDURE get_unpaid_invoices_for_client
	(
		client_id INT,
        OUT invoices_count INT,
        OUT invoices_total DECIMAL(9, 2)
	)
BEGIN
	SELECT COUNT(*), SUM(invoice_total)
    INTO invoices_count, invoices_total
    FROM invoices i
    WHERE i.client_id = client_id
		AND payment_total = 0;
END$$
DELIMITER ;

SET @invoices_count = 0;
SET @invoices_total = 0;
CALL get_unpaid_invoices_for_client(3, @invoices_count, @invoices_total);
SELECT @invoices_count, @invoices_total;
```

!> Better to avoid output parameters, because it will cause so much confusion and it is hard to use.
