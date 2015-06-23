# Classroom Exercises for Percona Training
###### Last Update: October 2014

## Exercise 5

In this exercise you will: Prepare the backup, start MySQL on DB2 and connect the slave.

* 5.1 - Decompress the backup
  
  If you didn't notice, we used the _--compress_ option on our innobackupex execution. So before we can do anything else to our DB2 instance, we have to decompress the backup.
  
  ```
  cd /var/lib/mysql
  innobackupex --decompress ./
  ```

* 5.2 - Prepare the backup
  
  When you take a backup of a running MySQL instance, there are usually transactions in-flight. Innobackupex records these transactions while taking the snapshot of your data on-disk. We must now apply all of those changes that happened during the backup process to the actual data files.
  
  In your MySQL-DB2 terminal window, you should still be inside your MySQL $DATADIR. We can now issue the following command:
  
  `innobackupex --apply-log --use-memory=4G ./`
  
  * --apply-log - This option tells innobackupex that we want to apply the changes.
  * --use-memory=4G - Increasing the amount of memory available to innobackupex can significantly speed up this process. If there is nothing else running on your system, you may increase this to 10/20/50GB.
   
  This is a two-stage process that innobackupex handles for you. Stage 1 applies all of the transactional information to the datafiles and stage 2 creates new logfiles. You should see "innobackupex: Completed OK!" again when both steps are completed. The backup is now prepared.

* 5.3 - Change ownership
  
  Presumably, you ran all of this as root. Thus we need to change the ownership of all the MySQL files otherwise it will not start.
  
  `chown -R mysql:mysql ./*`

* 5.4 - Start MySQL
  
  We can now start MySQL. The below example is on CentOS using SystemV init scripts. If you are running something different, refer to your OS documentation on how to start services.
  
  `/etc/init.d/mysql start`
  
  MySQL should now be running without any errors.

* 5.5 - Create a slave user
  
  On your MySQL-DB1 instance, we need to create a user that can connect and receive binlog events. This user needs the *REPLICATION SLAVE* privilege. Log in to MySQL and execute the following DDL statement.
  
  `GRANT REPLICATION SLAVE ON *.* TO 'replication'@'10.10.10.201' IDENTIFIED BY 'mypassword';`
  
  You can substitute any username, hostname and password you wish.

* 5.6 - Connect the slave instance and start replication
  
  Now that the user is created, go back to your MySQL-DB2 terminal and log in to MySQL. We now need to tell this instance to become a slave of DB1. We do that with the *CHANGE MASTER* command.
  
  `CHANGE MASTER TO MASTER_HOST='10.10.10.200', MASTER_USER='replication', MASTER_PASSWORD='mypassword', MASTER_LOG_FILE='master-bin.0000001', MASTER_LOG_POS=107;`

  To determine the values for MASTER_LOG_FILE and MASTER_LOG_POS, scroll up in your terminal window a bit. They will have been printed out at the end of the _--apply-log_ execution from exercise 5.
  
  You can now start the slave process.
  
  `START SLAVE;`
  
  You can now see the slave running with other information:
  
  `SHOW SLAVE STATUS\G`

* 5.7 - Woops!

  Doesn't look like our slave is running; we have an error! One thing we forgot to do is change the _server_id_ of the slave; it cannot be the same id as the master. Edit _/etc/my.cnf_ and change the server_id to anything other than 0 and it's current value.
  
  You will have to restart MySQL for this to take effect.
  
  After you've restarted MySQL, log back in and run another
  
  `SHOW SLAVE STATUS\G`

  If you see *Slave_SQL_Running: Yes* and *Slave_IO_Running: Yes*, congratulations! You've set up a slave instance!
