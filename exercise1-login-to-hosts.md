# Classroom Exercises for Percona Training
###### Last Update: January 2017

## Exercise 1

In this exercise, you will: Aquire the SSH configuration and SSH key and log in to your team's instances.

* 1.1 - Download the SSH configuration and key.
  1. Your instructor will provide a link to the SSH configuration file and key.
  2. Download them and save to your computer. It does not matter where you put them, as long as they are both in the same location.

* 1.2 - Fix the SSH key
  * If you are on Windows and using Putty, you will need the "Percona-Training.ppk".
  * Mac/Linux users, open a terminal window using Terminal/iTerm2 and type "chmod 600 Percona-Training.key". Windows users can ignore this step.

* 1.3 - Log in to your teams instances
  1. On Mac/Linux, type the following in the terminal window, replacing XXX with your team code and replacing N with your team number as designated by your instructor.

    `ssh -F ssh-config.txt Percona-Training-XXX-db2-TN`

    For example, if you are Team 5, in Houston, your command will look like this:

    `ssh -F ssh-config.txt Percona-Training-HOU-db2-T5`

    You may have to type 'Yes' to accept the new host key. Do so and press return.
    You should now be logged in to your team's MySQL-DB2 instance.
  2. On Windows, open an SSH connection to the IP address provided by your instructor using either the SSH key or PPK file, depending on your terminal application
