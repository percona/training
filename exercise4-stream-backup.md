# Classroom Exercises for Percona Training
###### Last Update: October, 2014

## Exercise 4

In this exercise you will: create a streaming backup of your MySQL-DB1

* 4.1 - Create the backup

  Open a new terminal window and log in to your MySQL-DB1 instance. Refer to exercise #1 as a reminder for the correct ssh syntax.

  Since we are streaming the backup to another host, it does not matter which directory you are currently at; your $HOMEDIR will suffice.
  
  This command will start innobackupex with appropriate options:
  
  `innobackupex --stream=xbstream --parallel=4 --slave-info --compress /tmp | nc db2-T5 3306`
  
  * --stream=xbstream - This option tells innobackupex to send the all of the backup to stdout. We catch this output with the "pipe" and redirect that output to netcat, which in turn, sends it to our other host on port 3306.
  * --parallel=4 - This option specifies how many parallel threads we can use to do the backup. If you are running in innodb_file_per_table=1 mode, this should speed up your backups.
  * --slave-info - This option will print out the master's binary log cordinates as the starting point for our slave configuration in the next exercise.
  * --compress - Compress the backup stream using simple LZ compression. May help if your backup must stream over a WAN.
  * /tmp - The last parameter is a location where innobackupex can create/store some temporary files during the backup process. They are removed automatically when the process is completed.
  
  In your MySQL-DB2 instance terminal window, you should see a line for each file as it is copied over and extracted.
  
  Once the backup is completed, you should see "innobackupex: Completed OK!" in your MySQL-DB1 terminal window.