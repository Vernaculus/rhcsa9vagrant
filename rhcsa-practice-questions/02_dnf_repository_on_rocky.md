***On Rocky***

# Configure the dnf repository for Rocky

### QUESTION #2:
Configure Your Rocky VM repository installed the packages distribution is available via DNF: \
    - baseos url=https://download.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os/ \
    - appstream url=https://download.rockylinux.org/pub/rocky/9/AppStream/x86_64/os/

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #2:

```
[root@rocky ~]# vim /etc/yum.repos.d/pkgs.repo

[BaseOS]
name=base
baseurl=https://download.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os/
gpgcheck=1
enabled=1

[AppStream]
name=app
baseurl=https://download.rockylinux.org/pub/rocky/9/AppStream/x86_64/os/
gpgcheck=1
enabled=1


:wq
```

* To confirm it is configured:
```
[root@rocky ~]# dnf clean all
[root@rocky ~]# dnf repolist all
```

* Both should be enabled

* SUCCESS!!
