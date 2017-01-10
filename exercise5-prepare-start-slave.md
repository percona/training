# Classroom Exercises for Percona Training
###### Last Update: August 2016

## Exercise 5

In this exercise you will: Prepare the backup, start MySQL on DB2 and connect the slave.

* 5.1 - Decompress the backup
  
  If you didn't notice, we used the _--compress_ option on our innobackupex execution. So before we can do anything else to our DB2 instance, we have to decompress the backup.
  
  ```
  cd /var/lib/mysql
  xtrabackup --decompress --parallel=4 --compress-threads=4 --target-dir=./
  
  # -- Optional --
  # find ./ -name "*.qp" -exec rm -f {} \;
  ```

* 5.2 - Prepare the backup
  
  When you take a backup of a running MySQL instance, there are usually transactions in-flight. Innobackupex records these transactions while taking the snapshot of your data on-disk. We must now apply all of those changes that happened during the backup process to the actual data files.
  
  In your MySQL-DB2 terminal window, you should still be inside your MySQL $DATADIR. We can now issue the following command:
  
  `xtrabackup --prepare --use-memory=4G --target-dir=./`
  
  * --prepare - This option tells xtrabackup that we want to apply any in-flight transactional changes.
  * --use-memory=4G - Increasing the amount of memory available to xtrabackup can significantly speed up this process. If there is nothing else running on your system, you may increase this to 10/20/50GB.
   
  This is a two-stage process that xtrabackup handles for you. Stage 1 applies all of the transactional information to the datafiles and stage 2 creates new logfiles. You should see "completed OK!" again when both steps are completed. The backup is now prepared and ready to be used.

* 5.3 - Change ownership
  
  Presumably, you ran all of this as root. Thus we need to change the ownership of all the MySQL files otherwise it will not start.
  
  `chown -R mysql:mysql ./*`

* 5.4 - Start MySQL
  
  We can now start MySQL. The below example is on CentOS using SystemV init scripts. If you are running something different, refer to your OS documentation on how to start services.
  
  `systemctl start mysql`
  
  MySQL should now be running without any errors.

* 5.5 - Create a slave user
  
  On your MySQL-DB1 instance, we need to create a user that can connect and receive binlog events. This user needs the *REPLICATION SLAVE* privilege. Log in to MySQL and execute the following DDL statement.
  
  ```
  CREATE USER 'replication'@'10.10.10.%' IDENTIFIED BY 'mypassword';
  GRANT REPLICATION SLAVE ON *.* TO 'replication'@'10.10.10.%';
  ```
  
  You can substitute any username, hostname and password you wish.

* 5.6 - Connect the slave instance and start replication
  
  Now that the user is created, go back to your MySQL-DB2 terminal and log in to MySQL. We now need to tell this instance to become a slave of DB1. We do that with the *CHANGE MASTER* command.
  
  `CHANGE MASTER TO MASTER_HOST='10.10.10.200', MASTER_USER='replication', MASTER_PASSWORD='mypassword', MASTER_LOG_FILE='master-bin.0000001', MASTER_LOG_POS=107;`

  To determine the values for MASTER_LOG_FILE and MASTER_LOG_POS, scroll up in your terminal window a bit. They will have been printed out at the end of the _--prepare_ execution from exercise 5. You can also _cat xtrabackup_binlog_pos_innodb_ to find this info.
  
  You can now start the slave process.
  
  `START SLAVE;`
  
  You can now see the slave running with other information:
  
  `SHOW SLAVE STATUS\G`

* 5.7 - Woops!

  Doesn't look like our slave is running; we have an error! One thing we forgot to do is change the _server_id_ of the slave; it cannot be the same id as the master. Edit _/etc/my.cnf_ and change the server_id to anything other than 0 and it's current value. You will also need to add _gtid-mode=ON_ and _enforce-gtid-consistency=ON_ since the master also has this enabled.
  
  You will have to restart MySQL for this to take effect.
  
  After you've restarted MySQL, log back in and run another
  
  `SHOW SLAVE STATUS\G`

  If you see *Slave_SQL_Running: Yes* and *Slave_IO_Running: Yes*, congratulations! You've set up a slave instance!
