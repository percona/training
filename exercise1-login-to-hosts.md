# Classroom Exercises for Percona Training
###### Last Update: October 2014

## Exercise 1

In this exercise, you will: Download the SSH configuration, Download the SSH key and Log in to your team instance

* 1.1 - Download the SSH configuration.
  1. Open your browser and go to _https://gist.github.com/utdrmac/95cc19fffc473f74944e_
  2. Click the 'Raw' button associated with the file "ssh-config-XXX.txt", where XXX is your team code (ex: CHI - Chicago, HOU - Houston, NYC - New York) and choose 'Save [File] As..' and save it to the home directory of your computer.

* 1.2 - Download the SSH key
  1. Open your browser and go to _https://gist.github.com/utdrmac/95cc19fffc473f74944e_
  2. Click the 'Raw' button associated with the file "Percona-Training.key", replacing XXX with your team code, and choose 'Save [File] As..' and save it to the home directory of your computer.
  3. If you are on Windows and using Putty, you need to download and use "Percona-Training.ppk". So download this file instead for Step 2.
  4. Mac/Linux users, open a terminal window using Terminal/iTerm2.
  5. Mac/Linux users, type "chmod 600 Percona-Training.key". Windows users can ignore this step.

* 1.3 - Log in to your team instance
  1. Type the following in the terminal window, again, replacing XXX with you team code and replacing N with your team number as designated by your instructor.

    `ssh -F ssh-config.txt Percona-Training-XXX-db2-TN`

    For example, if you are Team 5, in Houston, your command will look like this:

    `ssh -F ssh-config.txt Percona-Training-HOU-db2-T5`

    You may have to type 'Yes' to accept the new host key. Do so and press return.
    You should now be logged in to your team's MySQL-DB2 instance.
