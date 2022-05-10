# Lesson 8

## Home work 15-16

#### 1. Imagine you was asked to add new partition to your host for backup purposes. To simulate appearance of new physical disk in your server, please create new disk in Virtual Box (5 GB) and attach it to your virtual machine.
![image](https://user-images.githubusercontent.com/95025513/147134658-2a419e77-39e1-4ae8-9664-aa79f96f2141.png)
```bash
[root@localhost ~]# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0    8G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0    7G  0 part
  ├─centos-root 253:0    0  6.2G  0 lvm  /
  └─centos-swap 253:1    0  820M  0 lvm  [SWAP]
sr0              11:0    1 1024M  0 rom
[root@localhost ~]#
```

#### Also imagine your system started experiencing RAM leak in one of the applications, thus while developers try to debug and fix it, you need to mitigate OutOfMemory errors; you will do it by adding some swap space.
#### /dev/sdc - 5GB disk, that you just attached to the VM (in your case it may appear as /dev/sdb, /dev/sdc or other, it doesn't matter)
> Add 5 Gb disk
```bash
[sitis@localhost ~]$ lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0    8G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0    7G  0 part
  ├─centos-root 253:0    0  6.2G  0 lvm  /
  └─centos-swap 253:1    0  820M  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
sr0              11:0    1 1024M  0 rom
[sitis@localhost ~]$
```

#### 1.1. Create a 2GB   !!! GPT !!!   partition on /dev/sdc of type "Linux filesystem" (means all the following partitions created in the following steps on /dev/sdc will be GPT as well)
```bash
[root@localhost ~]# fdisk /dev/sdb
Command (m for help): g
Building a new GPT disklabel (GUID: DEC151C4-C051-43DC-AFD2-69BB5F155A7F)
Command (m for help): p

Disk /dev/sdb: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: gpt
Disk identifier: 2CAEC870-C614-46FE-8A8B-9409F6A58F46


#         Start          End    Size  Type            Name
 1         2048      4196351      2G  Linux filesyste
```
#### 1.2. Create a 512MB partition on /dev/sdc of type "Linux swap"
```bash
Command (m for help): t
Partition number (1,2, default 2): 2
Partition type (type L to list all types): 19
Changed type of partition 'Linux filesystem' to 'Linux swap'

Command (m for help): p

Disk /dev/sdb: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: gpt
Disk identifier: 2CAEC870-C614-46FE-8A8B-9409F6A58F46


#         Start          End    Size  Type            Name
 1         2048      4196351      2G  Linux filesyste
 2      4196352      5244927    512M  Linux swap

Command (m for help):
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
[root@localhost ~]# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0    8G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0    7G  0 part
  ├─centos-root 253:0    0  6.2G  0 lvm  /
  └─centos-swap 253:1    0  820M  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
├─sdb1            8:17   0    2G  0 part
└─sdb2            8:18   0  512M  0 part
sr0              11:0    1 1024M  0 rom

```
#### 1.3. Format the 2GB partition with an XFS file system
```bash
[root@localhost ~]# mkfs -t xfs  /dev/sdb1
meta-data=/dev/sdb1              isize=512    agcount=4, agsize=131072 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=524288, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@localhost ~]# parted /dev/sdb
GNU Parted 3.1
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print
Model: ATA VBOX HARDDISK (scsi)
Disk /dev/sdb: 5369MB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name  Flags
 1      1049kB  2149MB  2147MB  xfs
 2      2149MB  2685MB  537MB

(parted)

```

#### 1.4. Initialize 512MB partition as swap space
```bash
[root@localhost ~]# mkswap /dev/sdb2
Setting up swapspace version 1, size = 524284 KiB
no label, UUID=2186d7e8-2c1f-4b7f-9acb-d6083ee13a32
```

#### 1.5. Configure the newly created XFS file system to persistently mount at /backup
> Add a new line to the /etc/fstab
```bash
/dev/sdb1 /backup                               xfs     defaults        0 1
```

#### 1.6. Configure the newly created swap space to be enabled at boot
> Add a new line to the /etc/fstab
```bash
/dev/sdb2 none                                  swap    sw              0 0
```

#### 1.7. Reboot your host and verify that /dev/sdc1 is mounted at /backup and that your swap partition  (/dev/sdc2) is enabled
> Before reboot
```bash
[root@localhost ~]# free
              total        used        free      shared  buff/cache   available
Mem:        1882072      158232     1593180        8724      130660     1579296
Swap:        839676           0      839676
```
> After
```bash
[root@localhost ~]# free
              total        used        free      shared  buff/cache   available
Mem:        1882072      157244     1549140        8792      175688     1573768
Swap:       1363960           0     1363960
[root@localhost ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 908M     0  908M   0% /dev
tmpfs                    919M     0  919M   0% /dev/shm
tmpfs                    919M  8.6M  911M   1% /run
tmpfs                    919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root  6.2G  2.4G  3.9G  38% /
/dev/sda1               1014M  194M  821M  20% /boot
tmpfs                    184M     0  184M   0% /run/user/1000
[root@localhost ~]# df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 908M     0  908M   0% /dev
tmpfs                    919M     0  919M   0% /dev/shm
tmpfs                    919M  8.6M  911M   1% /run
tmpfs                    919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root  6.2G  2.4G  3.9G  38% /
/dev/sdb1                2.0G   33M  2.0G   2% /backup
/dev/sda1               1014M  194M  821M  20% /boot
tmpfs                    184M     0  184M   0% /run/user/1000
```
2. LVM. Imagine you're running out of space on your root device. As we found out during the lesson default CentOS installation should already have LVM, means you can easily extend size of your root device. So what are you waiting for? Just do it!
2.1. Create 2GB partition on /dev/sdc of type "Linux LVM"
```bash
Command (m for help): p

Disk /dev/sdb: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: gpt
Disk identifier: 8FF8C14A-F533-44CE-B6D2-65C2E888A341


#         Start          End    Size  Type            Name
 1         2048      4196351      2G  Linux filesyste
 2      4196352      5244927    512M  Linux swap
 3      5244928      9439231      2G  Linux LVM

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```

#### 2.2. Initialize the partition as a physical volume (PV)
```bash
[root@localhost ~]# pvcreate /dev/sdb3
  Physical volume "/dev/sdb3" successfully created.
```

#### 2.3. Extend the volume group (VG) of your root device using your newly created PV
```bash
[root@localhost ~]# pvs
  PV         VG     Fmt  Attr PSize  PFree
  /dev/sda2  centos lvm2 a--  <7.00g    0
  /dev/sdb3         lvm2 ---   2.00g 2.00g
[root@localhost ~]# vgextend centos /dev/sdb3
  Volume group "centos" successfully extended
[root@localhost ~]# pvs
  PV         VG     Fmt  Attr PSize  PFree
  /dev/sda2  centos lvm2 a--  <7.00g     0
  /dev/sdb3  centos lvm2 a--  <2.00g <2.00g

```

#### 2.4. Extend your root logical volume (LV) by 1GB, leaving other 1GB unassigned
```bash
[root@localhost ~]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao----  <6.20g
  swap centos -wi-ao---- 820.00m

[root@localhost ~]# lvextend -L+1G /dev/mapper/centos-root
  Size of logical volume centos/root changed from <6.20 GiB (1586 extents) to <7.20 GiB (1842 extents).
  Logical volume centos/root successfully resized.
[root@localhost ~]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao----  <7.20g
  swap centos -wi-ao---- 820.00m
```

#### 2.5. Check current disk space usage of your root device
```bash
[root@localhost ~]# df -h /
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  6.2G  2.4G  3.9G  38% /
[root@localhost ~]# df -h /
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  6.2G  2.4G  3.9G  38% /
```

#### 2.6. Extend your root device filesystem to be able to use additional free space of root LV
```bash
[root@localhost ~]# xfs_growfs /
meta-data=/dev/mapper/centos-root isize=512    agcount=4, agsize=406016 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=1624064, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 1624064 to 1886208
```

#### 2.7. Verify that after reboot your root device is still 1GB bigger than at 2.5.
```bash
[root@localhost ~]# df -h /
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  7.2G  2.4G  4.9G  33% /
```
