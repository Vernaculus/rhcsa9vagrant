***On Hail-NFS Server***

# The User List Needs to Be Disabled for Security Reasons

### QUESTION #24:
Disable the user list from showing up on the graphical user login (Gnome Display Manager) screen.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #24:

* Disable the user list:
```
[root@hail ~]# touch /etc/dconf/db/gdm.d/00-disable-user-list
[root@hail ~]# cat << EOF >> /etc/dconf/db/gdm.d/00-disable-user-list
[org/gnome/login-screen]
disable-user-list=true
EOF
[root@hail ~]# dconf update
[root@hail ~]# systemctl restart gdm
```

* SUCCESS!!
