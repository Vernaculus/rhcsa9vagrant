***On Rocky***

# Configure Chrony for Time Synchronization

### QUESTION #10:
Configure your system to synchronize the time to the Hail Workstation, ```hail.hailmary.local```. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #10:
* Install and configure ```chronyd``` on Rocky:
```
[root@rocky ~]# dnf install chrony
[root@rocky ~]# vim /etc/chrony.conf
```
* Comment out the 'pool' line and add this 'server' line below it:
```
#pool 2.rhel.pool.ntp.org iburst
server hail.hailmary.local iburst maxpoll 16
```
* Now SSH to Hail or open it up in another tab to open the firewall:
```
[root@hail ~]# firewall-cmd --permanent --add-service=ntp
success
[root@hail ~]# firewall-cmd --reload
success
```
* Now, go back to Rocky, and restart the Chronyd service and enable it so it starts at boot-time:
```
[root@rocky ~]# systemctl restart chronyd
[root@rocky ~]# systemctl enable chronyd
```
* Finally, to get time synchronization working:
```
[root@rocky ~]# timedatectl set-ntp true
```
* You may check your work by running:
```
[root@rocky ~]# chronyc sources -v

  .-- Source mode  '^' = server, '=' = peer, '#' = local clock.
 / .- Source state '*' = current best, '+' = combined, '-' = not combined,
| /             'x' = may be in error, '~' = too variable, '?' = unusable.
||                                                 .- xxxx [ yyyy ] +/- zzzz
||      Reachability register (octal) -.           |  xxxx = adjusted offset,
||      Log2(Polling interval) --.      |          |  yyyy = measured offset,
||                                \     |          |  zzzz = estimated error.
||                                 |    |           \
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* hail.hailmary.local      2   6   377    27    -15us[  -20us] +/-  205us
```

* SUCCESS!!

### Additional comment:

The old method of time synchronization in RHEL 6 was Ntpd.  This has been completely sunset by Chronyd since RHEL 7.  
