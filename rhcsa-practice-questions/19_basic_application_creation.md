***On Grace***

# Create a Very Basic Application

### QUESTION #19:
Build an application rhcsa that print the message when logged in as ablerate user: "Welcome to user ablerate."

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #19:

* Create the user if it doesn’t exist:
```
[root@grace ~]# useradd ablerate
[root@grace ~]# passwd ablerate
Changing password for user ablerate.
New password:
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@grace ~]#
```

* Here we make a script-style application in /usr/local/bin called "rhcsa":
```
[root@grace ~]# vim /usr/local/bin/rhcsa

#! /bin/bash
echo "Welcome to user ablerate"


:wq
```

* Next, we give executable permissions on that file:
```
[root@grace ~]# chmod +x /usr/local/bin/rhcsa
```

* Edit the user's ```/home/abelrate/.bashrc```:
```
[root@grace ~]# vim /home/ablerate/.bashrc

# User specific aliases and functions
if [ -d ~/.bashrc.d ]; then
    for rc in ~/.bashrc.d/*; do
        if [ -f "$rc" ]; then
            . "$rc"
        fi
    done
fi

/usr/local/bin/rhcsa


:wq
```

* Double-check the permissions:
```
[root@grace ~]# ls -l /usr/local/bin/rhcsa
-rwxr-xr-x. 1 root root 44 Apr 23 09:48 /usr/local/bin/rhcsa
```

* To check your work for the user’s welcome message:
```
[root@grace ~]# su - ablerate
Welcome to user ablerate.
```

* SUCCESS!!



