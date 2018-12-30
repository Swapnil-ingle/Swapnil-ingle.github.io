Various SQL implementations across the globe provides a way to tally the internal working of the query that the admin/user executes. There are some established parameters that results in the output query when we use EXPLAIN plan. Parameters like 
id, select_type, table, partitions, type, possible_keys, key, key_len, ref, etc. The resultant values in these parameters help the user to know the internal working (what look-up did the query perform,  how many rows where examined by the query, etc) of the query. 

To get to know more about explain plan I hosted an experiment and tested the result of the query with EXPLAIN plan. I tested scenarios in contrasts of using and not using indexes on a table. 

> "Indexes are critical to the performance of databases. Indexes become more important as data grows larger."

This is table *'actors'* that is the mice in this experiment.

```sql
CREATE TABLE actors ( 
  ID bigint(10), 
  Name varchar(20), 
  Address varchar(50)
);
```
