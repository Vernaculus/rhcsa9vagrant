***On Hail (NFS Server)***

# Setting Up Passwordless login from Hail to Rocky and Grace

### QUESTION #00:
Create SSH keys so that you can connnect to remote servers without a password.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #00:

* Create the user ansible ON ALL 3 systems:
```
[root@hail ~]# useradd -G wheel ansible; newgrp
[root@rocky ~]# useradd -G wheel ansible; newgrp
[root@grace ~]# useradd -G wheel ansible; newgrp
```

* Generate the key:
```
[root@hail ~]# ssh-keygen -t rsa -b 4096 -N ""
[root@hail ~]# for host in rocky grace; do ssh-copy-id -i ~/.ssh/id_rsa.pub $host; done
(Enter the ansible user's password for both nodes)
```

* Then test out login without a password:
```
[root@hail ~]# ssh ansible@rocky
[ansible@rocky ~]$ exit
[root@hail ~]# ssh ansible@grace
[ansible@grace ~]$ exit
[root@hail ~]# 
```

* SUCCESS!!

