# Using Transactions

## Using Distributed Transaction for Row Tables
Transactions specify an isolation level that defines the degree to which one transaction must be isolated from resource or data modifications made by other transactions. The transaction isolation levels define the type of locks acquired on read operations. Only one of the isolation level options can be set at a time, and it remains set for that connection until it is explicitly changed.

The following isolation levels are supported for row tables:

| Isolation level | Description |
|--------|--------|
|NONE|Default isolation level. The Database Engine uses shared locks to prevent other transactions from modifying rows while the current transaction is running a read operation. |
|READ_COMMITTED|SnappyData ensures that ongoing transactional as well as non-transactional (isolation-level NONE) operations never read uncommitted (dirty) data. SnappyData accomplishes this by maintaining transactional changes in a separate transaction state that are applied to the actual data-store for the table only at commit time. SnappyData detects only Write-Write conflicts while in READ_COMMITTED isolation level. </br>In READ COMMITTED, a read view is created at the start of each statement and lasts only as long as each statement execution.|
|REPEATABLE_READ|In this isolation level, a lock-based concurrency control DBMS implementation keeps read and write locks (acquired on selected data) until the end of the transaction. In REPEATABLE READ every lock acquired during a transaction is held for the duration of the transaction.|

For more information, see, [SET ISOLATION](../reference/sql_reference/set-isolation.md)

## Using Snapshot Isolation for Column Tables

Multi-Statement transactions are not supported on column tables. Instead, we provide snapshot isolation by default.  Snapshot ensures that all queries see the same version (snapshot), of the database, based on the state of the database at the moment in time when the query is executed. The snapshot is taken per statement for each partition, which means, the snapshot of the partition is taken the moment the query accesses the partition. This behavior is set by default for column tables and cannot be modified.
<!--Currently, only single statement snapshot isolation (that is, [autocommit](../reference/interactive_commands/autocommit.md) must be set to true) is supported.-->  

!!! Note:
	If you set the isolation level to `READ_COMMITTED` or `REPEATABLE_READ`, queries on column table report an error if [autocommit](../reference/interactive_commands/autocommit.md) is set to off (false). </br>Queries on column tables are supported when isolation level is set to `READ_COMMITTED` or `REPEATABLE_READ` and autocommit is set to **true**.