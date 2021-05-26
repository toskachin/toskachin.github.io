---
title: Linux system administrator从入门到放弃
layout: post
categories: Linux
tags: tech
excerpt: linux操作手册
---

- [system](#system)
  - [system information](#system-information)
    - [`hostnamectl`](#hostnamectl)
    - [`uname`](#uname)
    - [check user](#check-user)
  - [mix command](#mix-command)
    - [`sar`](#sar)
    - [`vmstat`](#vmstat)
    - [`top`](#top)
  - [`lsblk`](#lsblk)
  - [`lscpu`](#lscpu)
  - [`lshw`](#lshw)
  - [`lspci`](#lspci)
  - [`lsusb`](#lsusb)
  - [`uptime`](#uptime)
  - [`dmesg`](#dmesg)
  - [`strace`](#strace)
- [cpu](#cpu)
  - [`lscpu`](#lscpu-1)
  - [`vmstate`](#vmstate)
  - [`mpstat`](#mpstat)
  - [`pidstat`](#pidstat)
  - [`perf`](#perf)
- [memory](#memory)
  - [`free`](#free)
  - [`pamp`](#pamp)
  - [`dtrace`](#dtrace)
  - [`valgrind`](#valgrind)
- [process](#process)
  - [`ps`](#ps)
  - [`pidstat`](#pidstat-1)
  - [`pstree`](#pstree)
- [disk/IO](#diskio)
  - [Disk performance check](#disk-performance-check)
  - [`df`](#df)
  - [`du`](#du)
  - [`fsck`](#fsck)
  - [`iostat`](#iostat)
  - [`iotop`](#iotop)
  - [`pidstat`](#pidstat-2)
  - [`perf`](#perf-1)
  - [Disk add and extend or remove](#disk-add-and-extend-or-remove)
  - [`lsblk`](#lsblk-1)
  - [`fdisk`](#fdisk)
  - [physical volume](#physical-volume)
  - [volume group](#volume-group)
  - [Logical Volume](#logical-volume)
  - [扩展逻辑卷](#扩展逻辑卷)
  - [缩减逻辑卷](#缩减逻辑卷)
- [network](#network)
  - [route](#route)
    - [`mtr`](#mtr)
    - [`traceroute`](#traceroute)
    - [`ping`](#ping)
    - [`tcpdump`](#tcpdump)
    - [`netstat`](#netstat)
  - [dns](#dns)
    - [`nslookup`](#nslookup)
    - [`dig`](#dig)
    - [`iptable`](#iptable)
    - [`ss`](#ss)
  - [`fuser`](#fuser)
  - [`lsof`](#lsof)
  - [`nmap`](#nmap)
  - [`sar -n`](#sar--n)
- [files](#files)
  - [`less`](#less)
  - [`head`](#head)
  - [`tail`](#tail)
  - [`awk`](#awk)
  - [`sed`](#sed)
  - [`tr`](#tr)
  - [`find`](#find)
- [job control](#job-control)
  - [`bg` `fg`](#bg-fg)
  - [`disown`](#disown)
  - [`job`](#job)
  - [`nohup`](#nohup)
  - [`ps`](#ps-1)
  - [`pstree`](#pstree-1)
  - [`time`](#time)
  - [`watch`](#watch)
  - [`screen`](#screen)
- [users and permission](#users-and-permission)
  - [`chmod`](#chmod)
  - [`chown`](#chown)
- [VIM hotkeys](#vim-hotkeys)
  - [delete](#delete)
  - [move](#move)
- [linux_commands](#linux_commands)
  - [`scp`](#scp)
  - [`rsync`](#rsync)

# system
## system information
### `hostnamectl`
```bash
[root@sf-centos-101 ~]# hostnamectl
   Static hostname: sf-centos-101
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 67a52c5361b8494880a0be3cb79d1581
           Boot ID: 22199b0fc6724b20bb7c30e70fb5e17d
    Virtualization: vmware
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-1160.el7.x86_64
      Architecture: x86-64
```
### `uname`
```bash
[root@sf-centos-101 ~]# uname -a
Linux sf-centos-101 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```
### check user
```bash
[root@sf-centos-101 ~]# w root
 04:05:44 up 2 days, 19:06,  2 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    corp-sto-sha-01. 03:20   11:04   0.07s  0.07s -bash
root     pts/1    corp-sto-sha-01. 04:04    0.00s  0.03s  0.00s w root
[root@sf-centos-101 ~]# whoami
root
[root@sf-centos-101 ~]# users
root root
```
```bash
[root@sf-centos-101 ~]# who -a
           system boot  2021-04-03 08:59
LOGIN      tty1         2021-04-03 08:59               845 id=tty1
           run-level 3  2021-04-03 08:59
root     + pts/0        2021-04-06 03:20 00:11       39379 (corp-sto-sha-01.global.marinsoftware.com)
root     + pts/1        2021-04-06 04:04   .         39580 (corp-sto-sha-01.global.marinsoftware.com)
```

```bash
#check last user root login time
[root@sf-centos-101 ~]# last root
root     pts/1        corp-sto-sha-01. Tue Apr  6 04:04   still logged in
root     pts/0        corp-sto-sha-01. Tue Apr  6 03:20   still logged in
```
## mix command
### `sar`
```bash
-r  #memory
-S  #sawp
-d  # disk
-u  #cpu
-b #I/O activities
-n #network statistics sar -n DEV or sar -n UDP
-o #output sar 2 4 -o /tmp/data.log >/dev/null 2>$1
``` 
```bash
#history
[root@sf-centos-101 ~]# sar -r -f /var/log/sa/sa05
Linux 3.10.0-1160.el7.x86_64 (sf-centos-101) 	04/05/2021 	_x86_64_	(1 CPU)

12:00:01 AM kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
12:10:01 AM   1341824    521180     27.98      2252    171716    339432      9.58    171872     99364         0
12:20:01 AM   1341824    521180     27.98      2252    171724    339536      9.59    171940     99368         0
12:30:01 AM   1341528    521476     27.99      2252    171728    339608      9.59    172080     99364         0
12:40:01 AM   1341576    521428     27.99      2252    171732    339676      9.59    172128     99364         0
```
```bash
# memory
[root@sf-centos-101 ~]# sar -r 2 2
Linux 3.10.0-1160.el7.x86_64 (sf-centos-101) 	04/06/2021 	_x86_64_	(1 CPU)

03:30:11 AM kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
03:30:13 AM   1322376    540628     29.02      2252    174900    354680     10.01    190772     99372        20
03:30:15 AM   1322376    540628     29.02      2252    174900    354680     10.01    190780     99372        20
Average:      1322376    540628     29.02      2252    174900    354680     10.01    190776     99372        20
```
### `vmstat`
```bash
-d #I/O disk
-a #By default vmstat doesn’t display this information. Use option -a, to display active and inactive memory information
```
### `top`
```bash
m #memory
P #CPU
```


## `lsblk`
## `lscpu`
## `lshw`
## `lspci`
## `lsusb`
## `uptime`
## `dmesg`
## `strace`


# cpu
## `lscpu`
## `vmstate`
```bash
-w #format
[root@sf-centos-101 ~]# vmstat -w 2 2
procs -----------------------memory---------------------- ---swap-- -----io---- -system-- --------cpu--------
 r  b         swpd         free         buff        cache   si   so    bi    bo   in   cs  us  sy  id  wa  st
 2  0            0      1322380         2252       220916    0    0     1     0   34   55   0   0 100   0   0
 0  0            0      1322380         2252       220948    0    0     0     0   54   93   0   0 100   0   0
```
## `mpstat`
## `pidstat`
## `perf`

# memory
## `free`
```bash
[root@sf-centos-101 ~]# free -mh
              total        used        free      shared  buff/cache   available
Mem:           1.8G        309M        1.3G        9.4M        217M        1.3G
Swap:          1.6G          0B        1.6G
```
## `pamp`
## `dtrace`
## `valgrind`


# process
## `ps`
## `pidstat`
```bash
-d     #Report I/O statistics
-r     #memory
-u     #cpu

```
## `pstree`


# disk/IO
## Disk performance check
## `df`
```bash
[root@sf-centos-101 ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 899M     0  899M   0% /dev
tmpfs                    910M     0  910M   0% /dev/shm
tmpfs                    910M  9.4M  901M   2% /run
tmpfs                    910M     0  910M   0% /sys/fs/cgroup
/dev/mapper/centos-root  214G  2.2G  212G   2% /
/dev/sda1               1014M  154M  861M  16% /boot
tmpfs                    182M     0  182M   0% /run/user/0
```
## `du`
```bash
[root@sf-centos-101 ~]# du -sh /root
36K	/root

du -sh * | sort -hr | head -n 5 #list top 5 biggest folders
```
## `fsck`
## `iostat`
## `iotop`
## `pidstat`
## `perf`
## Disk add and extend or remove
## `lsblk`
```bash
[root@sf-centos-101 ~]# lsblk
NAME            MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda               8:0    0    16G  0 disk
├─sda1            8:1    0     1G  0 part /boot
└─sda2            8:2    0    15G  0 part
  ├─centos-root 253:0    0 213.4G  0 lvm  /
  └─centos-swap 253:1    0   1.6G  0 lvm  [SWAP]
sdb               8:16   0   200G  0 disk
└─centos-root   253:0    0 213.4G  0 lvm  /
sr0              11:0    1   4.4G  0 rom
```
## `fdisk`
```bash
fdisk -l
fdisk /dev/sdb
```
## physical volume

```bash
pvs #简要pv信息显示
pvdisplay 显示pv的详细信息
pvcreated /dev/DEVICE #创建pv
```
## volume group
```bash
vgs
vgdisplay
vgcreate  [-s #[kKmMgGtTpPeE]] VolumeGroupName  PhysicalDevicePath [PhysicalDevicePath...]
vgextend  VolumeGroupName  PhysicalDevicePath [PhysicalDevicePath...]
vgreduce  VolumeGroupName  PhysicalDevicePath [PhysicalDevicePath...]
  先做pvmove
vgremove

```

## Logical Volume
```bash
lvs
lvdisplay

lvcreate -L #[mMgGtT] -n NAME VolumeGroup

lvremove /dev/VG_NAME/LV_NAME
```
## 扩展逻辑卷
```bash
CDISK=/dev/sdX
pvcreate ${CDISK}
pvdisplay #
vgdisplay #List Volume group
#Extend the vg
vgextend vg1 ${CDISK}
vgs --unit m  #list free disk
lvextend -r -L +(Number from VFree cloumn) /dev/mapper/vg1-root # -r resize
OR
lvextend -r -l +100%FREE /dev/mapper/vg1-root
```
## 缩减逻辑卷
```bash
umount /dev/VG_NAME/LV_NAME
e2fsck -f /dev/VG_NAME/LV_NAME
resize2fs /dev/VG_NAME/LV_NAME #[mMgGtT]
lvreduce -L [-]#[mMgGtT] /dev/VG_NAME/LV_NAME
mount
```


# network
## route
### `mtr`
```bash
"r" to generate the mtr in a report format
"w" to show the full hostname of each 'hop' where available
"c X" to indicate to send X number packets. By default, an MTR report will only send 10. We recommend 50, which will take around a minute. 

[root@sf-centos-101 ~]# mtr -rwc 50 google.com
Start: Tue Apr  6 22:55:25 2021
HOST: sf-centos-101                 Loss%   Snt   Last   Avg  Best  Wrst StDev
  1.|-- 10.0.5.2                       0.0%    50    0.2   0.2   0.1   0.3   0.0
  2.|-- host241.right-net.com          0.0%    50    0.4   0.9   0.4  13.7   2.2
  3.|-- ae7.cr1.sjc2.us.zip.zayo.com   0.0%    50    2.0   2.2   1.9  10.2   1.1
  4.|-- ae27.cs1.sjc2.us.eth.zayo.com  0.0%    50    2.1   2.4   2.0   8.5   0.9
  5.|-- ???                           100.0    50    0.0   0.0   0.0   0.0   0.0
  6.|-- 142.250.160.46                 0.0%    50    2.4   2.5   2.4   3.3   0.0
  7.|-- 108.170.243.1                  0.0%    50    3.6   3.4   3.0   4.4   0.0
  8.|-- 108.170.243.14                 0.0%    50    5.3   3.3   2.4  16.2   2.1
  9.|-- 142.250.234.135                0.0%    50    2.8   3.1   2.7   5.4   0.5
 10.|-- 142.250.237.172                0.0%    50    9.9  10.5   9.8  27.3   2.5
 11.|-- 142.250.235.172               40.0%    50   50.7  51.0  50.6  55.2   0.8
 12.|-- 142.250.232.69                90.0%    50   50.8  50.7  50.5  50.8   0.0
 13.|-- 216.239.57.137                86.0%    50   66.9  67.1  66.9  67.9   0.0
 14.|-- 108.170.227.149                0.0%    50   67.6  67.4  67.2  67.7   0.0
 15.|-- 108.170.248.97                 0.0%    50   66.8  66.8  66.7  67.2   0.0
 16.|-- 142.250.235.239                0.0%    50   67.4  67.6  67.2  69.7   0.3
 17.|-- lga34s33-in-f14.1e100.net      0.0%    50   66.6  66.6  66.6  66.9   0.0

```
### `traceroute`
```bash
traceroute google.com
```

### `ping`
```bash
ping -4 google.com #use ipv4
ping -6 google.com #use ipv6
ping -c 2 google.com #Specify the Number of Packets
ping -s 40 -c 5 google.com #control package size
```
### `tcpdump`
### `netstat`
```bash
netstat -antp 

```
## dns
### `nslookup`
```bash
nslookup dnsserver google.com #指定dns服务器解析
```
### `dig`
```bash
dig @dnsserver youtube.com #dig @8.8.8.8 youtube.com

[root@sf-centos-101 ~]# dig +short youtube.com
172.217.2.14

[root@sf-centos-101 ~]# dig +nocmd -x 8.8.8.8  +noall +answer
8.8.8.8.in-addr.arpa.	20558	IN	PTR	dns.google.

#cat domains.txt
#lxer.com
#linuxtoday.com
#tuxmachines.org

dig -f domains.txt +short

#108.166.170.171
#70.42.23.121
#204.68.122.43
```
### `iptable`
### `ss`



## `fuser`
## `lsof`
## `nmap`

## `sar -n`





# files
## `less`
## `head`
## `tail`
## `awk`
## `sed`
## `tr`
## `find`

# job control
## `bg` `fg`
## `disown`
## `job`
## `nohup`
## `ps`
## `pstree`
## `time`
## `watch`
## `screen`

# users and permission
## `chmod`
## `chown`

# VIM hotkeys
```bash
vim     + line number    file  
vim +81 /Users/rjin/.ssh/known_hosts

```
## delete
```bash
x        删除当前光标下的字符
dw       删除光标之后的单词剩余部分。
d$       删除光标之后的该行剩余部分。
dd       删除当前行。

cw     删除光标后一个单词，进入instert mode
c$
c        功能和d相同，区别在于完成删除操作后进入INSERT MODE
cc       也是删除当前行，然后进入INSERT MODE
```
## move
```bash
nw  n为数字  光标向后N个单词
nb  光标向前N个单词

gg

G

0  #绝对行首
^  #行首非第一个非字符
$  #绝对行


h或退格: 左移一个字符；
l或空格: 右移一个字符；
j: 下移一行；
k: 上移一行

ctrl+d: 下翻半屏。
ctrl+u: 上翻半屏。
ctrl+e: 向下滚动一行。
ctrl+y: 向上滚动一行。


nw  n为数字  光标向后N个单词
nb  光标向前N个单词

a  #光标后字符插入
A  #行尾插入


gg  #移到首行
G   #移到末尾


d$   #删除光标到行尾


u/U #撤销操作


0  #绝对行首
^  #行首非第一个非字符
$  #绝对行尾




：set nu   #显示行数

```


# linux_commands
## `scp`
## `rsync`

