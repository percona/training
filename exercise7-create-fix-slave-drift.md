# Classroom Exercises for Percona Training
###### Last Update: October 6th, 2014

## Exercise 7

In this exercise you will: intentionally remove data from the slave, re-checksum the cluster and fix the drift.

* Remove data from the slave

Under normal production environments, your slaves are ran in read-only mode (if they aren't, fix this!). There are two ways you can override the read-only flag: replication and any user with SUPER privilege.

Let's log in to our MySQL-DB2 instance and remove some data. We can do this because we have the SUPER privelege on our 'root' account.

`mysql-db2> DELETE FROM company_name WHERE id BETWEEN 10000 AND 11000;`

This should have remove 1,001 rows from the table.

* Detect slave drift

Now, let's log back in to our MySQL-DB1 instance and re-run our checksum. If we suspect a single table might have some problems in it, we can checksum just that one table to save some time.

`pt-table-checksum  h=localhost -d imdb -t company_name -u checksum -pchecksum1 --no-check-binlog-format --no-version-check`

You'll notice the command is almost the same as the previous exercise but with the addition of *-d* and *-t* which allow you to specify a single database and table or multiple databases and tables in a comma-separated list.

You should have seen this as your output:

```
            TS ERRORS  DIFFS     ROWS  CHUNKS SKIPPED    TIME TABLE
10-11T15:26:02      0      1   241457       5       0   3.661 imdb.company_name
```

As you can see, under the *DIFFS* column, it shows 1, indicating 1 of the 5 chunks had a problem on a slave.

* Fixing slave drift

Now that we have successfully detected the drift, we need to fix it. We do that with another Percona Toolkit tool named *pt-table-sync*.

pt-table-checksum saves all of the chunk/checksum information to a table called _checksums_ in the _percona_ database. pt-table-sync is able to read this data and isolate which chunk is bad and then do a row-by-row comparison to find that specific row (or rows) that may be missing or have incorrect column-level values.

Let's use pt-table-sync to first display the SQL necessary to fix the slave. First, log back in to your MySQL-DB2 instance.

