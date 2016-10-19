# Classroom Exercises for Percona Training
###### Last Update: October 2014

## Exercise 4

In this exercise you will: create a streaming backup of your MySQL-DB1

* 4.1 - Create the backup

  Open a new terminal window and log in to your MySQL-DB1 instance. Refer to exercise #1 as a reminder for the correct ssh syntax.

  Since we are streaming the backup to another host, it does not matter which directory you are currently at; your $HOMEDIR will suffice.
  
  This command will start innobackupex with appropriate options:
  
  `xtrabackup --backup --stream=xbstream --compress --compress-threads=4 --parallel=4 --no-version-check | nc db2-T5 3306`
  
  * --backup - This option says to do a backup.
  * --stream=xbstream - This option tells xtrabackup to send the all of the backup data to stdout. We catch this output with the "pipe" and redirect that output to netcat, which in turn, sends it to our other host on port 3306.
  * --parallel=4 - This option specifies how many parallel threads we can use to do the backup. If you are running in innodb_file_per_table=1 mode, this should speed up your backups.
  * --compress - Compress the backup stream using simple LZ compression. May help if your backup must stream over a WAN.
  * --compress-threads - Since we are backing up parallel, it will help to also compress in parallel
  * --no-version-check - Simply prevents xtrabackup from calling home to check if there is a newer version of the binary
  
  In your MySQL-DB2 instance terminal window, you should see a line for each file as it is copied over and extracted.
  
  Once the backup is completed, you should see "innobackupex: Completed OK!" in your MySQL-DB1 terminal window.
