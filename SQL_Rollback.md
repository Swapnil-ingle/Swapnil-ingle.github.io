## SQL rollback section

Whenever we try to perform any statement in SQL it directly affects SQL rollback section. SQL statements such as Insert, Update or Delete when executed make a change in the relation. These changes has to be stored until the transaction lives; that is from **begin** to **commit**. When we say changes we mean the state of tuple before the particular operation was performed.

### Why are the changes stored?

This is a way to maintain the atomicity property of databases. Either full transaction should be showed in effect or none. Hence, if for some reason - may it be hardware failure, power outages or software issue - a transaction fails the half-done effect of the transaction has to be reverted. This should bring back the database in the state before the transaction actually happened. These temporary changes, called as logs, reside in the SQL rollback section as per the SQL standards however, although the gist is same the actual implementation of SQL rollback defers from vendor to vendor. MySQL has *Undo logs* and *Redo logs* which work the magic to maintain the consistency and atomicity of the database.

> "An undo log is a collection of undo log records associated with a single transaction. An undo log record contains information about how to undo the latest change by a transaction to a clustered index record. If another transaction needs to see the original data as part of a consistent read operation, the unmodified data is retrieved from undo log records. Undo logs exist within undo log segments, which are contained within rollback segments. Rollback segments reside in the system tablespace, undo tablespaces, and in the temporary tablespace."

