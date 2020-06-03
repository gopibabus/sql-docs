# 🔥Events

?> Event is a task (or a block of sql code) that gets executed according to a schedule. It is used to automate database maintenance tasks.

### ✳Creating an Event

```sql
-- check global variables start with "event"
SHOW VARIABLES LIKE 'event%';
-- Switch on event_schedular
SET GLOBAL event_schedular = ON;
-- Creating an event
DELIMITER $$
CREATE EVENT yearly_delete_stale_audit_rows
ON SCHEDULE
	EVERY 1 YEAR STARTS '2019-01-01' ENDS '2029-01-01'
DO BEGIN
	DELETE FROM payments_audit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$
DELIMITER ;
```

### ✳Viewing an Event

```sql
SHOW EVENTS;
SHOW EVENTS LIKE 'yearly%';
```

### ✳Dropping an Event

```sql
DROP EVENT IF EXISTS yearly_delete_stale_audit_rows;
```

### ✳Altering an Event

```sql
DELIMITER $$
ALTER EVENT yearly_delete_stale_audit_rows
ON SCHEDULE
	EVERY 1 YEAR STARTS '2019-01-01' ENDS '2029-01-01'
DO BEGIN
	DELETE FROM payments_audit
    WHERE action_date < NOW() - INTERVAL 1 YEAR;
END $$
DELIMITER ;

-- Enable Event
ALTER EVENT yearly_delete_stale_audit_rows ENABLE;

-- Disable Event
ALTER EVENT yearly_delete_stale_audit_rows DISABLE;
```
