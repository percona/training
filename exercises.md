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

    `ssh -F ssh-config.txt Percona-Training-XXX-db1-TN`

    For example, if you are Team 5, in Houston, your command will look like this:

    `ssh -F ssh-config.txt Percona-Training-HOU-db1-T5`

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
    
    This will install, amoung others, the following packages thanks to the wildcard in the command above:
    
    * Percona-Server-client-56.x86_64
    * Percona-Server-server-56.x86_64
    * Percona-Server-shared-56.x86_64
    
  3. Now we can install the Percona Toolkit and other essential tools and libraries.

    `yum install percona-toolkit percona-xtrabackup nc -y`

## Exercise 3
