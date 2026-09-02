## UPDATED 2025-03-13:


<br><br>
***On Grace***

# Lab #15: Logical Volume Management (LVM)

## Objective:
Create and manage Logical Volumes using parted

### QUESTION #15.1:
Create a Logical Volume named LV1 of size 8GB using the extra storage provided.  Here is what you will learn:
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/47206a5d-73f3-483e-b9d6-e3f35aea26ab)

DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.1:
* First, if you are running virtual machines, you will need to add an additional storage device here...6GB,5GB, and 4GB "Hard Disks"
* You can shut down the virtual machine and then add the additional storage as needed.
### Step 1: Verify Attached Storage
```bash
[root@grace ~]# lsblk
NAME           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sro              11:0    1 1024M  0 rom
vda            252:0    0   10G  0 disk
├─vda1         252:1    0    1G  0 part /boot
├─vda2         252:2    0    9G  0 part
│ ├─_rhe1_192-root 253:0 0  8G  0 lvm  /
│ └─_rhe1_192-swap 253:1 0  1G  0 lvm  [SWAP]
vdb            252:16   0    6G  0 disk
vdc            252:17   0    5G  0 disk
vdd            252:18   0    4G  0 disk
```
### Step 2: Create Physical Volumes
No partition table is needed here. These three disks are dedicated entirely to LVM, so `pvcreate` writes LVM metadata straight to each raw disk.
```bash
[root@grace ~]# pvcreate /dev/vdb /dev/vdc /dev/vdd
# We can verify that they are available:
[root@grace ~]# pvs
PV           VG      Fmt    Attr    PSize   PFree
/dev/vdb             lvm2   ---     6.00g   6.00g
/dev/vdc             lvm2   ---     5.00g   5.00g
/dev/vdd             lvm2   ---     4.00g   4.00g
```
### Step 3: Create Volume Group
```bash
[root@grace ~]# vgcreate VG1 /dev/vdb /dev/vdc /dev/vdd
  Volume group "VG1" successfully created
# Verify the new VG1 has been created:
[root@grace ~]# vgs
VG   #PV  #LV #SN Attr     VSize   VFree
VG1  3    0   0   wz--n-   14.99g  14.99g
```
### Step 4: Create Logical Volume (LV1) using 8GB
```bash
[root@grace ~]# lvcreate -L 8G -n LV1 VG1
  Logical volume "LV1" created.
# Verify:
[root@grace ~]# lvs
LV     VG     Attr      LSize  Pool Origin Data Metax Move Log Cy Sync Convert
LV1    VG1    wi-a--    8.00g
```
### Step 5: Format and Mount Logical Volume
```bash
[root@grace ~]# mkfs.xfs /dev/VG1/LV1
[root@grace ~]# mkdir /lv
```
### Step 6: Automate Mounting with `/etc/fstab`
```bash
[root@grace ~]# echo "/dev/VG1/LV1 /lv xfs defaults,nodev 0 0" >> /etc/fstab
```
### Step 7: Instantly mount the LVM without a reboot
```bash
[root@grace ~]# mount -a
```
* SUCCESS!!

<br><br><br>
### QUESTION #15.2:
Extend the Logical Volume you created, LV1 by 2GB. 
<br/><br/>
Again, DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.2:

### Step 1: Extend Logical Volume by 2GB
Check available space first:
```bash
[root@grace ~]# vgs
VG   #PV  #LV #SN   Attr     VSize   VFree
VG1  2    1   0     wz--n-   10.99g  2.99g
```

>If space permits on VG1 with the 2 physical disks:
```bash
[root@grace ~]# lvextend -r -L +2G /dev/VG1/LV1

# Verify:
[root@grace ~]# lvs
```

### Step 2: Extend Volume Group VG1 to /dev/sdd
If you need more space:
```bash
[root@grace ~]# vgextend VG1 /dev/vdd
[root@grace ~]# lvextend -r -L +2G /dev/VG1/LV1
```

### Step 3: Verify New Size
```bash
[root@grace ~]# df -h
```

* SUCCESS!!

### QUESTION #15.3:
Create a Logical Volume named LV2 with 10 extent where the size of each extent is 8MB.
<br/><br/>
DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.3:

**Step 1: Check Current Volume Group Configuration**

```bash
[root@grace ~]# vgdisplay
```

**Step 2: If current PE size isn't 8MB, recreate the volume group:**

```bash
[root@grace ~]# umount -l /lv
[root@grace ~]# lvchange -an /dev/VG1/LV1
[root@grace ~]# lvremove /dev/VG1/LV1
[root@grace ~]# vgremove VG1
```

**Step 3: Create a new Volume Group with 8MB extents:**

```bash
[root@grace ~]# vgcreate -s 8MB VG1 /dev/vdb
[root@grace ~]# vgdisplay VG1
```

**Step 4: Create Logical Volume `LV2` with 10 extents:**

```bash
[root@grace ~]# lvcreate -l 10 -n LV2 VG1
[root@grace ~]# lvs
```

Your Logical Volume `LV2` is now set up successfully with custom extent size.

* SUCCESS!!

<br><br><br>
### Troubleshooting:
Remove existing configuration if required and recreate:
```bash
[root@grace ~]# umount -l /lv
[root@grace ~]# lvchange -an /dev/VG1/LV1
[root@grace ~]# lvremove /dev/VG1/LV1
[root@grace ~]# vgremove VG1
[root@grace ~]# vgcreate -s 8MB VG1 /dev/vdb
[root@grace ~]# vgdisplay VG1
```

---

## Conclusion
You've successfully configured Logical Volume Management using parted, managing and adjusting logical volumes as required.
