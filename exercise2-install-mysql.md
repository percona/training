# Classroom Exercises for Percona Training
###### Last Update: August 2016

## Exercise 2

In this exercise, you will: Install the Percona Yum repository, Percona Server 5.7, Percona Toolkit and other useful utilities.

* 2.1 - Install the Percona Yum Repo
  1. Open your browser and go to _http://www.percona.com/doc/percona-server/5.7/installation/yum_repo.html_
  2. Copy the line inside the first tan box under 'Automatic Installation'. The line begins with *yum install*. Copy the entire line.
  3. Open your terminal window and log in to your MySQL-DB2 instance if you closed it earlier.
  4. Paste the copied line from the step above and press return. This will download and install the Percona Yum repository information.
  5. Verify installation by typing the following:

    `yum list | grep Percona`
    
    You should see many items scroll across the screen. This is an indication the repo was installed and can be accessed.

* 2.2 - Install Extra Packages for Enterprise Linux (EPEL) Repo
  1. This repo is required to install *libev* which is required for *Percona Xtrabackup 2.4*
  
    `yum install epel-release -y`

* 2.3 - Install Percona Server 5.7
  1. Your terminal window should still be open from the above step. Let's continue.
  2. Type the following to install Percona Server 5.7 via yum

    `yum install Percona-Server-*57 -y`
    
    This will install, among others, the following packages thanks to the wildcard in the command above:
    
    * Percona-Server-client-57.x86_64
    * Percona-Server-server-57.x86_64
    * Percona-Server-shared-57.x86_64
    
  3. Now we can install the Percona Toolkit and other essential tools and libraries. 'nc' is the 'netcat' command line utility for doing quick transfers between servers.

    `yum install percona-toolkit percona-xtrabackup-24 qpress nc -y`
