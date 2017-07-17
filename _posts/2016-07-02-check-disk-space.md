---
layout: post
title: "Find out what is using your disk space"
excerpt: When you are running low on disk you can use the following methods to drill down to the directory where you can start erasing files.
categories: [administration]
comments: true
---

When you are running low on disk you can use the following methods to drill down to the directory where you can start erasing files.

The command df -h will show whether the files are on one partition or more than one.  

```bash

df -h

john@DESKTOP-G2KE893:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
rootfs          466G  167G  299G  36% /
data            466G  167G  299G  36% /data
cache           466G  167G  299G  36% /cache
mnt             466G  167G  299G  36% /mnt
none            466G  167G  299G  36% /dev
none            466G  167G  299G  36% /run
none            466G  167G  299G  36% /run/lock
none            466G  167G  299G  36% /run/shm
none            466G  167G  299G  36% /run/user
C:              466G  167G  299G  36% /mnt/c
D:              1.9T  534G  1.3T  29% /mnt/d
root            466G  167G  299G  36% /root
home            466G  167G  299G  36% /home

```

Sometimes when you run df -h you see plenty of free disk space but you still cannot save a file. In a file system, each file is linked to an inode and limited number will have been created when the partition was made. If you have lots of small files you may use up the available inodes. You can run the command df -i to check.

```bash

john@DESKTOP-G2KE893:~$ df -i
Filesystem     Inodes   IUsed   IFree IUse% Mounted on
rootfs            999 -999001 1000000     - /
data              999 -999001 1000000     - /data
cache             999 -999001 1000000     - /cache
mnt               999 -999001 1000000     - /mnt
none              999 -999001 1000000     - /dev
none              999 -999001 1000000     - /run
none              999 -999001 1000000     - /run/lock
none              999 -999001 1000000     - /run/shm
none              999 -999001 1000000     - /run/user
C:                999 -999001 1000000     - /mnt/c
D:                999 -999001 1000000     - /mnt/d
root              999 -999001 1000000     - /root
home              999 -999001 1000000     - /hom
```

The du command will help you work out which directories are using the most space. Pipe this to a sort command to help you further.


```bash
john@nas:/$ sudo du -s * | sort -n
0       dev
0       initrd.img
0       initrd.img.old
0       proc
0       sys
0       vmlinuz
0       vmlinuz.old
4       lib64
4       mnt
4       srv
8       media
12      webmin-setup.out
16      lost+found
28      tmp
696     opt
700     root
12740   bin
13944   sbin
40156   run
69040   etc
113702  boot
597504  lib
2008228 usr
7609948 var
1228339992      home
```

Here we see that the home directory is using the most filespace so cd /home and run the du command again to drill down further.

The ncdu command which is not built in to Linux but is found in most distributions will give you a friendly graphic interface using ncurses.

```bash

. 940.6 GiB [##########] /home                                                                                              1.9 GiB [          ] /usr
.   1.1 GiB [          ] /var
  583.5 MiB [          ] /lib
. 111.0 MiB [          ] /boot
.  39.1 MiB [          ] /run
   13.6 MiB [          ] /sbin
   12.4 MiB [          ] /bin
.   9.0 MiB [          ] /etc
  696.0 KiB [          ] /opt
   28.0 KiB [          ] /tmp
!  16.0 KiB [          ] /lost+found
   12.0 KiB [          ]  webmin-setup.out
    8.0 KiB [          ] /media
    4.0 KiB [          ] /lib64
e   4.0 KiB [          ] /srv
!   4.0 KiB [          ] /root
e   4.0 KiB [          ] /mnt
.   0.0   B [          ] /proc
.   0.0   B [          ] /sys
    0.0   B [          ] /dev
@   0.0   B [          ]  initrd.img.old
@   0.0   B [          ]  initrd.img
@   0.0   B [          ]  vmlinuz.old
@   0.0   B [          ]  vmlinuz

```
