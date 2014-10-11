# Classroom Exercises for Percona Training
###### Last Update: October 6th, 2014

## Exercise 7

In this exercise you will: intentionally remove data from the slave, re-checksum the cluster and fix the drift.

#### 7.1 Remove Data from the Slave

Under normal production environments, your slaves are ran in read-only mode (if they aren't, fix this!). There are two ways you can override the read-only flag: replication and any user with SUPER privilege.

Let's log in to our MySQL-DB2 instance and remove some data. We can do this because we have the SUPER privelege on our 'root' account.

`mysql-db2> DELETE FROM company_name WHERE id BETWEEN 10000 AND 11000;`

This should have remove 1,001 rows from the table.

#### 7.2 Detect Slave Drift

Now, let's log back in to our MySQL-DB1 instance and re-run our checksum. If we suspect a single table might have some problems in it, we can checksum just that one table to save some time.

`pt-table-checksum  h=localhost -d imdb -t company_name -u checksum -pchecksum1 --no-check-binlog-format --no-version-check`

You'll notice the command is almost the same as the previous exercise but with the addition of **-d** and **-t** which allow you to specify a single database and table or multiple databases and tables in a comma-separated list.

You should have seen this as your output:

```
            TS ERRORS  DIFFS     ROWS  CHUNKS SKIPPED    TIME TABLE
10-11T15:26:02      0      1   241457       5       0   3.661 imdb.company_name
```

As you can see, under the __DIFFS__ column, it shows 1, indicating 1 of the 5 chunks had a problem on a slave.

#### 7.3 Fixing Slave Drift

Now that we have successfully detected the drift, we need to fix it. We do that with another Percona Toolkit tool named __pt-table-sync__.

pt-table-checksum saves all of the chunk/checksum information to a table called __checksums__ in the __percona__ database. pt-table-sync is able to read this data and isolate which chunk is bad and then do a row-by-row comparison to find that specific row (or rows) that may be missing or have incorrect column-level values.

Let's use pt-table-sync to first display the SQL necessary to fix the slave. First, log back in to your MySQL-DB2 instance.

`pt-table-sync --replicate percona.checksums h=localhost,u=checksum,p=checksum1 --print | wc -l`

The output redirect (ie: pipe) to *w*ord *c*ount is optional. It serves as a quick check that the tool will execute 1001 SQL statements (ie: the number of rows we deleted originally) to fix the slave. Run the command above but remove everything after the pipe character.

`pt-table-sync --replicate percona.checksums h=localhost,u=checksum,p=checksum1 --print`

This will print 1001 REPLACE statements. You use _--print_ to sanity check what will be executed on the master. 

Remember that last part: __executed on the master__

We __NEVER__ execute directly on the slave. We always execute on the master and let the changes flow through replication. Because the master already has this data, the command is considered a no-op (no operation) and doesn't incur any significant load on the master. The DML still flows thru replication down to all slaves.

Run the tool again, this time specifying __--execute__ to actually execute each statement.

`pt-table-sync --replicate percona.checksums h=localhost,u=checksum,p=checksum1 --execute --verbose`

You should see output similar to this:

```
# DELETE REPLACE INSERT UPDATE ALGORITHM START    END      EXIT DATABASE.TABLE
#      0    1001      0      0 Chunk     18:16:45 18:16:46 2    imdb.company_name
```

We can see here that 1001 REPLACE statements were executed. Let's verify the checksum again.

#### 7.4 Verify Fixed Drift

Let's run another checksum on just this table and verify our __pt-table-sync__ worked.

```
pt-table-checksum  h=localhost -d imdb -t company_name -u checksum -pchecksum1 --no-check-binlog-format --no-version-check
            TS ERRORS  DIFFS     ROWS  CHUNKS SKIPPED    TIME TABLE
10-11T18:18:48      0      0   241457       5       0   2.670 imdb.company_name
```

Excellent! Our checksum now show no differences between our master and our slave.
