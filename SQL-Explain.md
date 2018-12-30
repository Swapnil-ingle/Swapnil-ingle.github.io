Various SQL implementations across the globe provide a way to tally the internal working of the query that the admin/user executes. There are some established parameters that result in the output query when we use the EXPLAIN plan. Parameters like 
id, select_type, table, partitions, type, possible_keys, key, key_len, ref, etc. The resultant values in these parameters help the user to know the internal working (what look-up did the query perform,  how many rows were examined by the query, etc) of the query. 

To get to know more about explain plan I hosted an experiment and tested the result of the query with EXPLAIN plan. I tested scenarios in contrasts of using and not using indexes on a table. 

> "Indexes are critical to the performance of databases. Indexes become more important as data grows larger."

This is the table *'actors'* that is the mice in this experiment.

```sql
CREATE TABLE actors ( 
  ID bigint(10), 
  Name varchar(20), 
  Address varchar(50)
);
```

I created a procedure that populates the *actors* table with dummy data of 5000 rows. After populating the table with sample data, I selected data from *actors* using *WHERE ID='<int>'*. 

```sql
SELECT * FROM actors WHERE ID=545;  
```

Explain clause clearly shows the internal working of this query. **\G** is used to get a well-formatted output.

```sql
EXPLAIN SELECT * FROM actors WHERE ID=545\G;
```

**Output:**

```sql
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: actors
   partitions: NULL
         type: ALL
possible_keys: NULL
          key: NULL
      key_len: NULL
          ref: NULL
         rows: 5000
     filtered: 10.00
        Extra: Using where
1 row in set, 1 warning (0.00 sec)
```

Let's understand the output: 
1. The *id* is output row number, *select_type* is 'SIMPLE' here there are many types ex; SIMPLE, PRIMARY, UNION, DEPENDENT UNION, etc., 
2. table is the relation name, 
3. partitions are 'The partitions from which records would be matched by the query.', 
4. type is what type of join is used, 
5. possible_keys is a column indicating the indexes from which MySQL can choose to find the rows in this table. Note that this column is totally independent of the order of the tables as displayed in the output from EXPLAIN. *(That means that some of the keys in possible_keys might not be usable in practice with the generated table order.)*, 
6. the 'key' is the key that SQL actually uses, 
7. key_len column indicates the length of the key that MySQL decided to use. The value of key_len enables you to determine how many parts of a multiple-part key MySQL actually uses. If the key column says NULL, 
8. The ref column shows which columns or constants are compared to the index named in the key column to select rows from the table, 
9. The rows column indicates the number of rows MySQL believes it must examine to execute the query, 
10. The filtered column indicates an estimated percentage of table rows that will be filtered by the table condition. The maximum value is 100, which means no filtering of rows occurred. Values decreasing from 100 indicate increasing amounts of filtering. rows show the estimated number of rows examined and rows × filtered shows the number of rows that will be joined with the following table. For example, if rows are 1000 and filtered is 50.00 (50%), the number of rows to be joined with the following table is 1000 × 50% = 500, 
11. The 'extra' column contains additional information about how MySQL resolves the query. For descriptions of the different values.

*[Refer this link for more info on Explain output](https://dev.mysql.com/doc/refman/5.5/en/explain-output.html)*

The output of the query clearly suggests that SQL estimated to examine 5000 rows to get the output of the query we executed. This is doing the full table scan of the relation actors. This is, of course, not at all efficient. 

This is due to the lack of appropriate use of *indexes*. we'll see how the output drastically changes after the addition of an index.

```sql
CREATE INDEX actors_table_idx ON actors(ID, Name, Address);

EXPLAIN SELECT * FROM actors WHERE id=545\G;
```

**OUTPUT:**

```sql
*************************** 1. row ***************************
           id: 1
  select_type: SIMPLE
        table: actors
   partitions: NULL
         type: ref
possible_keys: actors_table_idx
          key: actors_table_idx
      key_len: 8
          ref: const
         rows: 1
     filtered: 100.00
        Extra: Using index
1 row in set, 1 warning (0.00 sec)
```

From the output, we can clearly see that SQL this time round examined only 1 row in contrast to the prior full table scan. Also, we can see that output attributes such as key, possible_keys, key_len, ref were also changed. Key was 'actors_table_idx' as this was the index used.

## The results
of the experiment are two-fold. We learned how important indexes are to the internal execution of any query and saw a simple example of how to leverage them and how to use EXPLAIN clause to learn the internal routine of our query executions.
