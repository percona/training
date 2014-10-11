# Classroom Exercises for Percona Training
###### Last Update: October 2014

## Exercise 3

In this exercise you will: prepare your MySQL-DB2 instance to become a slave

* 3.1 - Prepare MySQL-DB2

  Because we installed MySQL from the yum repository in Exercise 2, our data directory (by default _/var/lib/mysql_) now has basic information in it. We need to remove this because the backup/snapshot of our DB1 instance will contain some of these files. It is best to remove it before starting the process.
    
    `rm -rf /var/lib/mysql/*`
    
  Now that the data is gone, we need to start _netcat_ in listen mode to receive the incomming backup stream from DB1 when we start it. Because port 3306 (the default MySQL port) must be open for normal MySQL operations, we are listening on that same port just in case firewalls or iptables are blocking other ports.
    
    `nc -l 3306 | xbstream -vx`
    
  After pressing return on the above command, you will be presented with a newline. This is good and means _nc_ is listening and waiting for data. Leave this terminal window open.
