# 🔥Concurrency

?> Database **concurrency** is the ability of the database to support multiple users and processes working on the database concurrently. For example, an airline reservation system supporting thousands of active users at any given time!

?> By default in **MYSQL** when two same transactions are executed against a given database. The first transaction will create lock on rows and other transaction should wait till lock is released by first transaction.

## ⚡Concurrency Problems

### ✳Lost Updates

?>Two applications, A and B, might both read the same row and calculate new values for one of the columns based on the data that these applications read. If A updates the row and then B also updates the row, A's update lost.

### ✳Access to uncommitted data.

?> Application A might update a value, and B might read that value before it is committed. Then, if A backs out of that update, the calculations performed by B might be based on invalid data.

### ✳Non-repeatable data.

?> Application A might read a row before processing other requests. In the meantime, B modifies or deletes the row and commits the change. Later, if A attempts to read the original row again, it sees the modified row or discovers that the original row has been deleted.

### ✳Phantom reads.

?> Application A might execute a query that reads a set of rows based on some search criterion. Application B inserts new data or updates existing data that would satisfy application A's query. Application A executes its query again, within the same unit of work, and some additional (phantom) values are returned.

> [Reference](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.admin.perf.doc/doc/c0005267.html)
