# Classroom Exercises for Percona Training
###### Last Update: October 6th, 2014

## Exercise 1

In this exercise, you will: Download the SSH configuration, Download the SSH key and Log in to your team instance

* 1.1 - Download the SSH configuration.
  1. Open your browser and go to _http://learning.percona.com/_  Log in with your credentials. If you do not know your credentials, use the "Forgot Password" link to reset them.
  2. Click 'My Courses' on the right-hand side and find this class.
  3. Scroll down to 'Class Materials'
  4. Right-click on "ssh-config.txt" and choose 'Save [File] As..' and save it to the home directory of the lab computer.

* 1.2 - Download the SSH key
  1. Log in to _http://learning.percona.com/_ if you are not logged in still from the above step.
  2. Right-click on "Percona-Training-XXX.key" where XXX is your city code (ex: CHI - Chicago, HOU - Houston, NYC - New York) and choose 'Save [File] As..' and save it to the home directory of the lab computer.
  3. Close/minimize your browser window.
  4. Open a terminal window. This will place you in the home directory of the lab user.
  5. Type "chmod 600 Percona-Training-XXX.key", again, replacing XXX with your city code, then press _return_.

* 1.3 - Log in to your team instance
  1. Type the following in the terminal window, again, replacing XXX with you city code and replacing N with your team number as designated by your instructor.

    `ssh -F ssh-config.txt Percona-Training-XXX-db2-TN`

    For example, if you are Team 5, in Houston, your command will look like this:

    `ssh -F ssh-config.txt Percona-Training-HOU-db2-T5`

    You may have to type 'Yes' to accept the new host key. Do so and press return.
    You should now be logged in to your team's MySQL-DB2 instance.