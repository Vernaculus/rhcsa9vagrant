***On Hail-NFS Server***

# Configure the NFS Server Share Directory (This will help you for Lab #7's autofs question):

* First, install the required packages:
```
[root@hail ~]# dnf install -y nfs* rpc*
```

* Start and Enable the service:
```
[root@hail ~]# systemctl enable --now nfs-server
[root@hail ~]# systemctl status nfs-server
```

* Setup the shared directory: 
```
[root@hail ~]# mkdir -p /ourhome/remoteuserx
[root@hail ~]# mkdir /shared
[root@hail ~]# chmod 2770 /shared
```

* Configure the ```/etc/exports``` file for sharing with Rocky:
```
[root@hail ~]# vim /etc/exports

/ourhome/remoteuserx 192.168.99.0/24(rw,sync,no_root_squash)


:wq
```

* Export the nfs-shared directory:
```
[root@hail ~]# exportfs -avr
```

* Configure the firewall:
```
[root@hail ~]# firewall-cmd --add-service={nfs,mountd,rpc-bind} --permanent
[root@hail ~]# firewall-cmd --reload
```

* Create ```remoteuserx``` for Lab #7:
```
[root@hail ~]# groupadd -g 1234 autofsusers
[root@hail ~]# useradd -d /ourhome/remoteuserx -u 1234 -g autofsusers remoteuserx  
[root@hail ~]# passwd remoteuserx
```
