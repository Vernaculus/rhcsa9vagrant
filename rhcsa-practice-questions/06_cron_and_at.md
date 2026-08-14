***On Rocky***

# Add entry to cron

### Question #6 Part 1:
Use Cron to setup two scheduled tasks:
- Set a Cron Job for the user "Natasha" which runs daily every minute local time, and executes "Ex200 Testing" with logger.
- Set another Cron Job for the user "Nastasha" which runs daily at 2:30pm local time, and executes "Hello World".

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #6:
* You may reference the syntax setting up cron tasks by running:
```
[root@rocky ~]# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```  

* Run the crontab command for user natasha:
```
[root@rocky ~]# which logger
/bin/logger
[root@rocky ~]# crontab -eu natasha

* * * * * /usr/bin/logger "Ex200 Testing"


:wq
```

* Let's set another cron job an echo "Hello World" message which will go off at 2:30pm every day:
```
[root@rocky ~]# crontab -eu natasha

* * * * * /bin/logger "Ex200 Testing"
30 14 * * * /bin/echo "Hello World"

:wq 
```

* Now let's see what cron jobs are setup under user natasha:
```
[root@rocky ~]# crontab -lu natasha
* * * * *    /usr/bin/logger "Ex200 Testing"
30 14 * * * /bin/echo "Hello World"
```

* SUCCESS!!


### Setting Up the at Command
The at command is used to schedule commands to be executed at a particular time in the future. Here’s how to install and enable it:

* Install the at package:
```
[root@rocky ~]# dnf install -y at
[root@rocky ~]# systemctl enable --now atd
```

* Schedule a task to run five minutes from the current time. It will create a text file in /tmp with the content "Hello, world!":
```
[root@rocky ~]# echo "echo 'Hello, world!' > /tmp/hello.txt" | at now + 5 minutes
```

* To list scheduled jobs:
```
[root@rocky ~]# atq
```

* View a specific job:
```
[root@rocky ~]# at -c <job number>
```

* Remove a scheduled job:
```
[root@rocky ~]# atrm <job number>
```
* For more information:
```
man at
```

* SUCCESS!!


### Additional comments:

* The log file does not need to exist - it will be created automatically. \
* Of course **root** user can edit crontab of every user with flag **-u** **specifying user name**. \
* It is worth remembering that there are **/etc/cron.allow** and **/etc/cron.deny**. 
