***On Rocky***
#  Search for string using grep and redirect the output

### QUESTION #12:
Find all strings "ich" from "/usr/share/dict/words" file and copy those strings in a /root/lines file. 

This file can be installed using 
```
sudo dnf -y install words
```

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #12:
* The ```grep``` command provides what is needed:
```
[root@rocky ~]# grep ich /usr/share/dict/words > /root/lines
```

SUCCESS!!
