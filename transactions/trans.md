# 🔥Transactions

?> **Transactions** are **groups of SQL statements** that represent a single unit of work.

?> Transactions should respect **ACID** properties.

?> **A**(Atomicity)**C**(Consistency)**I**(Isolation)**D**(Durability)

<img alt="acid" width="700px" src="/assets/images/acid.png" />

```sql
START TRANSACTION;

INSERT INTO orders (customer_id, order_id, status)
VALUES (1, '2019-01-01', 1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(), 1, 1, 1);

-- End of transaction
COMMIT;
-- if you want to undo changes
ROLLBACK;
```

?> Each individual database interaction (i.e., each SQL statement) submitted through the database connection in **autocommit** mode will be executed in its own transaction that is implicitly committed.

```sql
-- In order to see autocommit status
SHOW VARIABLES LIKE 'autocommit';
```
