***On Grace***

# Setting up a Grub Password to prevent bootloader modification

### QUESTION #23:
Set a user password on the Grub bootloader to prevent the root password from being reset.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #23:

* Set the Grub bootloader password and then reboot:
```
[root@grace ~]# grub2-setpassword
Enter password:
Confirm password:
[root@grace ~]# init 6
```

* To rollback the Grub bootloader password (make it blank):
```
[root@grace ~]# find /boot -name "*user.cfg" -exec sed -i '/GRUB2_PASSWORD/d' {} \;
[root@grace ~]# init 6
```

### That will effectively blank the password and you will simply have to run init 6 to reboot.


* SUCCESS!!
