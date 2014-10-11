# Classroom Exercises for Percona Training
###### Last Update: October 2014

## Exercise 6

In this exercise you will learn: How to checksum a table

* 6.1 - What is a Checksum?
  
  If you don't know what a checksum is, you can read up on the details: https://en.wikipedia.org/wiki/Checksum 
  In a nutshell, you pass arbitrary amounts of data into a hashing function that returns a value. This value is the checksum.
  If you pass the same exact data into the function again, you will get the same result.
  
  However, if even a single bit (0 or 1) is different, the resultant checksum will not match the earlier checksum.
  This is how you determine if two sets of data are the same. If the checksums don't match, the data doesn't match.
  
* 6.2 - Checksumming a table - Built-In
  
  There is a built-in MySQL command to checksum a table:
  
  `CHECKSUM TABLE foo;`
  
  However, this is not recommend as you only get 1 answer at the end and MySQL must process all the rows in the table in one large batch. This could take hours depending on the size of your table.
  
* 6.3 - Checksumming a table - pt-table-checksum
  
  _pt-table-checksum_ is a free tool provided by Percona. http://www.percona.com/doc/percona-toolkit/pt-table-checksum.html
  This script generates checksums in a much more efficient way. Instead of operating on the entire table, pt-table-checksum operates on "chunks" of rows. The default size of 1 chunk is 1000 rows.
  You execute pt-table-checksum to checksum your entire database (all tables in all databases) via:
  
  `pt-table-checksum -u user -p pass h=localhost`
  
  That's it! The script will go table by table and calculate the checksum of each "chunk". The script is very adaptive and tries to do as much as possible in the shortest amount of time.
  
  By default, that time is 0.5 seconds. So, if MySQL is able to checksum 1000 rows in under 0.5s, the script will increase this chunk size for the next chunk and continue to do so until it can no longer do the calculations within 0.5s.
  This way, the load on your server stays low.
  
  Since we have a Master->Slave setup from the first group of exercises, let's take full advantage of pt-table-checksum.
  
* 6.4 - Create checksum user.
  
  Let's create a user that can login to both machines. This user will be used in our call to the script. pt-table-checksum can then log in to the slave(s) and monitor replication lag and pause itself if lag gets too high. We also need to create a *percona* database to store the checksum results.
  
  ```
  CREATE DATABASE percona;
  GRANT SELECT, SUPER, PROCESS, REPLICATION SLAVE ON *.* TO 'checksum'@'%' IDENTIFIED BY 'checksum1';
  GRANT SELECT, SUPER, PROCESS ON *.* TO 'checksum'@'localhost' IDENTIFIED BY 'checksum1';
  GRANT ALL ON percona.* TO 'checksum'@'%' IDENTIFIED BY 'checksum1';
  ```
  
  We created a separate @localhost user for the socket connection we will make on the master running the tool.
  Additionally, both users require SUPER and PROCESS priveleges to change their session to STATEMENT mode for replication purposes and to gather information about connected slaves. ALL is granted on the 'percona' database so that the tool can create the checksum table, or empty it and write to it.
  
* 6.5 - Calculate the Checksums
  
  Now that we have set up our user, let's run a checksum on all of our data:
  
  ```
  pt-table-checksum  h=localhost -u checksum -p checksum1 \
       --no-check-binlog-format --no-version-check
  ```
  
  You should see output similar to this:
  
  ```
              TS ERRORS  DIFFS     ROWS  CHUNKS SKIPPED    TIME TABLE
   10-11T00:49:22      0      0   633135       7       0   6.717 imdb.aka_name
  ```
  
  We can see the timestamp of completion of this table, if there were any errors during the checksum, if any differences were found (# of chunks different), how many rows in the table, how many chunks the script created out of those rows, if any chunks got skipped, how long it took to checksum the table and the table name.
  
  Congratulations! You've checksum'd your entire database!
