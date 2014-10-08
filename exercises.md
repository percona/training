# Classroom Exercises for Percona Training
###### Last Update: October 6th, 2014

## Exercise 1

In this exercise, you will: Download the SSH configuration, Download the SSH key and Log in to your team instance

* Download the SSH configuration.
  1. Open your browser and go to _http://learning.percona.com/_  Log in with your credentials. If you do not know your credentials, use the "Forgot Password" link to reset them.
  2. Click 'My Courses' on the right-hand side and find this class.
  3. Scroll down to 'Class Materials'
  4. Right-click on "ssh-config.txt" and choose 'Save [File] As..' and save it to the home directory of the lab computer.

* Download the SSH key
  1. Log in to _http://learning.percona.com/_ if you are not logged in still from the above step.
  2. Right-click on "Percona-Training-XXX.key" where XXX is your city code (ex: CHI - Chicago, HOU - Houston, NYC - New York) and choose 'Save [File] As..' and save it to the home directory of the lab computer.
  3. Close/minimize your browser window.
  4. Open a terminal window. This will place you in the home directory of the lab user.
  5. Type "chmod 600 Percona-Training-XXX.key", again, replacing XXX with your city code, then press _return_.

* Log in to your team instance
  1. Type the following in the terminal window, again, replacing XXX with you city code and replacing N with your team number as designated by your instructor.

    `ssh -F ssh-config.txt Percona-Training-XXX-db2-TN`

    For example, if you are Team 5, in Houston, your command will look like this:

    `ssh -F ssh-config.txt Percona-Training-HOU-db2-T5`

    You may have to type 'Yes' to accept the new host key. Do so and press return.
    You should now be logged in to your team's MySQL-DB2 instance.


## Exercise 2

In this exercise, you will: Install the Percona Yum repository, Percona Server 5.6, Percona Toolkit and other useful utilities.

* Install the Percona Yum Repo
  1. Open your browser and go to _http://www.percona.com/doc/percona-server/5.6/installation/yum_repo.html_
  2. Copy the line inside the first tan box under 'Automatic Installation'. The line begins with *yum install*. Copy the entire line.
  3. Open your terminal window and log in to your MySQL-DB2 instance if you closed it earlier.
  4. Paste the copied line from the step above and press return. This will download and install the Percona Yum repository information.
  5. Verify installation by typing the following:

    `yum list | grep Percona`
    
    You should see many items scroll across the screen. This is an indication the repo was installed and can be accessed.

* Install Percona Server 5.6
  1. Your terminal window should still be open from the above step. Let's continue.
  2. Type the following to install Percona Server 5.6 via yum

    `yum install Percona-Server-*56 -y`
    
    This will install, among others, the following packages thanks to the wildcard in the command above:
    
    * Percona-Server-client-56.x86_64
    * Percona-Server-server-56.x86_64
    * Percona-Server-shared-56.x86_64
    
  3. Now we can install the Percona Toolkit and other essential tools and libraries. 'nc' is the 'netcat' command line utility for doing quick transfers between servers.

    `yum install percona-toolkit percona-xtrabackup nc -y`

## Exercise 3

In this exercise you will: prepare your MySQL-DB2 instance to become a slave

* Prepare MySQL-DB2

  Because we installed MySQL from the yum repository in Exercise 2, our data directory (by default _/var/lib/mysql_) now has basic information in it. We need to remove this because the backup/snapshot of our DB1 instance will contain some of these files. It is best to remove it before starting the process.
    
    `rm -rf /var/lib/mysql/*`
    
  Now that the data is gone, we need to start _netcat_ in listen mode to receive the incomming backup stream from DB1 when we start it. Because port 3306 (the default MySQL port) must be open for normal MySQL operations, we are listening on that same port just in case firewalls or iptables are blocking other ports.
    
    `nc -l 3306 | xbstream -vx`
    
  After pressing return on the above command, you will be presented with a newline. This is good and means _nc_ is listening and waiting for data. Leave this terminal window open.

## Exercise 4

In this exercise you will: create a backup of your MySQL-DB1 instance and stream that backup to your MySQL-DB2 instance.

* Create the backup

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

## Exercise 5
