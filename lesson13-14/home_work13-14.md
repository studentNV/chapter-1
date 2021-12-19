# Lesson 6

## Home work 13-14

### Repositories and Packages

#### - Use rpm for the following tasks:
#### 1. Download sysstat package.
> rpm can only download and immediately install using the -i flag, but just how to download the pack neither in a person nor in your lectures or anywhere else I have not found if it is somewhere, let me read it, I will be grateful
```bash
[sitis@localhost lesson7]$ yumdownloader sysstat
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.axelname.ru
 * extras: mirror.axelname.ru
 * updates: mirror.axelname.ru
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
sysstat-10.1.5-19.el7.x86_64.rpm  
```
#### 2. Get information from downloaded sysstat package file.
```bash
[sitis@localhost lesson7]$ rpm -qi sysstat
Name        : sysstat
Version     : 5.0.5
Release     : 1
Architecture: x86_64
Install Date: Sun 19 Dec 2021 02:07:07 PM MSK
Group       : Applications/System
Size        : 314518
License     : GPL
Signature   : DSA/SHA1, Wed 20 Oct 2004 09:51:59 PM MSD, Key ID b44269d04f2a6fd2
Source RPM  : sysstat-5.0.5-1.src.rpm
Build Date  : Wed 30 Jun 2004 06:17:04 PM MSD
Build Host  : dolly.build.redhat.com
Relocations : (not relocatable)
Packager    : Red Hat, Inc. <http://bugzilla.redhat.com/bugzilla>
Vendor      : Red Hat, Inc.
URL         : http://perso.wanadoo.fr/sebastien.godard/
Summary     : The sar and iostat system monitoring commands.
Description :
This package provides the sar and iostat commands for Linux. Sar and
iostat enable system monitoring of disk, network, and other IO
activity.
```

#### 3. Install sysstat package and get information about files installed by this package.
```bash
[sitis@localhost lesson7]$ sudo rpm -ivh sysstat-10.1.5-19.el7.x86_64.rpm --nodeps
Preparing...                          ################################# [100%]
        file /etc/cron.d/sysstat from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /etc/sysconfig/sysstat from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/bin/iostat from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/bin/mpstat from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/bin/sar from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/lib64/sa/sa1 from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/lib64/sa/sa2 from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/lib64/sa/sadc from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/af/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/de/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/es/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/fr/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/it/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/ja/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/pl/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/pt/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/ro/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/ru/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/locale/sk/LC_MESSAGES/sysstat.mo from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/man/man1/iostat.1.gz from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/man/man1/mpstat.1.gz from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/man/man1/sar.1.gz from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/man/man8/sa1.8.gz from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/man/man8/sa2.8.gz from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
        file /usr/share/man/man8/sadc.8.gz from install of sysstat-10.1.5-19.el7.x86_64 conflicts with file from package sysstat-5.0.5-1.x86_64
[sitis@localhost lesson7]$ rpm -ql sysstat
/etc/cron.d/sysstat
/etc/rc.d/init.d/sysstat
/etc/sysconfig/sysstat
/usr/bin/iostat
/usr/bin/mpstat
/usr/bin/sar
/usr/lib64/sa
/usr/lib64/sa/sa1
/usr/lib64/sa/sa2
/usr/lib64/sa/sadc
/usr/share/doc/sysstat-5.0.5
/usr/share/doc/sysstat-5.0.5/CHANGES
/usr/share/doc/sysstat-5.0.5/COPYING
/usr/share/doc/sysstat-5.0.5/CREDITS
/usr/share/doc/sysstat-5.0.5/README
/usr/share/doc/sysstat-5.0.5/TODO
/usr/share/locale/af/LC_MESSAGES/sysstat.mo
/usr/share/locale/de/LC_MESSAGES/sysstat.mo
/usr/share/locale/es/LC_MESSAGES/sysstat.mo
/usr/share/locale/fr/LC_MESSAGES/sysstat.mo
/usr/share/locale/it/LC_MESSAGES/sysstat.mo
/usr/share/locale/ja/LC_MESSAGES/sysstat.mo
/usr/share/locale/nb_NO/LC_MESSAGES/sysstat.mo
/usr/share/locale/nn_NO/LC_MESSAGES/sysstat.mo
/usr/share/locale/pl/LC_MESSAGES/sysstat.mo
/usr/share/locale/pt/LC_MESSAGES/sysstat.mo
/usr/share/locale/ro/LC_MESSAGES/sysstat.mo
/usr/share/locale/ru/LC_MESSAGES/sysstat.mo
/usr/share/locale/sk/LC_MESSAGES/sysstat.mo
/usr/share/man/man1/iostat.1.gz
/usr/share/man/man1/mpstat.1.gz
/usr/share/man/man1/sar.1.gz
/usr/share/man/man8/sa1.8.gz
/usr/share/man/man8/sa2.8.gz
/usr/share/man/man8/sadc.8.gz
/var/log/sa
```

#### - Add NGINX repository (need to find repository config on https://www.nginx.com/) and complete the following tasks using yum:
> File /etc/yum.repos.d/nginx.repo
```bash
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

```bash
[sitis@localhost lesson7]$ sudo yum-config-manager --enable nginx-mainline
```

#### 1. Check if NGINX repository enabled or not.
```bash
[sitis@localhost lesson7]$ yum repolist enabled |grep nginx
nginx-mainline/7/x86_64                nginx mainline repo                   875
nginx-stable/7/x86_64                  nginx stable repo                     256
```

#### 2. Install NGINX.
```bash
[sitis@localhost lesson7]$ sudo yum install nginx
```

#### 3. Check yum history and undo NGINX installation.
```bash
[sitis@localhost lesson7]$ sudo yum history list
Loaded plugins: fastestmirror
ID     | Login user               | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     8 | sitis <sitis>            | 2021-12-19 14:53 | Install        |    1 E<
     7 | sitis <sitis>            | 2021-12-16 19:23 | Install        |    4 >
     6 | sitis <sitis>            | 2021-12-11 02:13 | Install        |    1
     5 | sitis <sitis>            | 2021-12-04 21:07 | I, U           |  107
     4 | sitis <sitis>            | 2021-12-04 15:51 | Install        |    1
     3 | sitis <sitis>            | 2021-12-01 20:51 | Install        |   31
     2 | sitis <sitis>            | 2021-11-30 19:49 | Install        |    2
     1 | System <unset>           | 2021-11-25 19:05 | Install        |  299
history list
[sitis@localhost lesson7]$ sudo yum history info  8
Loaded plugins: fastestmirror
Transaction ID : 8
Begin time     : Sun Dec 19 14:53:22 2021
Begin rpmdb    : 340:9039a73ba03da00b01af4619d8e3966e62997cf4
End time       :                           (0 seconds)
End rpmdb      : 341:57d7047dd84218234e49aa1afbb660ce18a76eb4
User           : sitis <sitis>
Return-Code    : Success
Command Line   : install nginx
Transaction performed with:
    Installed     rpm-4.11.3-48.el7_9.x86_64                      @updates
    Installed     yum-3.4.3-168.el7.centos.noarch                 @anaconda
    Installed     yum-plugin-fastestmirror-1.1.31-54.el7_8.noarch @anaconda
Packages Altered:
    Install nginx-1:1.21.4-1.el7.ngx.x86_64 @nginx-mainline
Scriptlet output:
   1 ----------------------------------------------------------------------
   2
   3 Thanks for using nginx!
   4
   5 Please find the official documentation for nginx here:
   6 * https://nginx.org/en/docs/
   7
   8 Please subscribe to nginx-announce mailing list to get
   9 the most important news about nginx:
  10 * https://nginx.org/en/support.html
  11
  12 Commercial subscriptions for nginx are available on:
  13 * https://nginx.com/products/
  14
  15 ----------------------------------------------------------------------
history info
[sitis@localhost lesson7]$ yum history undo 8
Loaded plugins: fastestmirror
You need to be root to perform this command.
[sitis@localhost lesson7]$ sudo yum history undo 8
Loaded plugins: fastestmirror
Undoing transaction 8, from Sun Dec 19 14:53:22 2021
    Install nginx-1:1.21.4-1.el7.ngx.x86_64 @nginx-mainline
Resolving Dependencies
--> Running transaction check
---> Package nginx.x86_64 1:1.21.4-1.el7.ngx will be erased
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================
 Package              Arch                  Version                             Repository                      Size
=====================================================================================================================
Removing:
 nginx                x86_64                1:1.21.4-1.el7.ngx                  @nginx-mainline                2.8 M

Transaction Summary
=====================================================================================================================
Remove  1 Package

Installed size: 2.8 M
Is this ok [y/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Erasing    : 1:nginx-1.21.4-1.el7.ngx.x86_64                                                                   1/1
  Verifying  : 1:nginx-1.21.4-1.el7.ngx.x86_64                                                                   1/1

Removed:
  nginx.x86_64 1:1.21.4-1.el7.ngx

Complete!
```

#### 4. Disable NGINX repository.
```bash
[sitis@localhost lesson7]$ sudo yum-config-manager --disable nginx-\*
```

#### 5. Remove sysstat package installed in the first task.
```bash
[sitis@localhost lesson7]$ sudo yum remove sysstat
Loaded plugins: fastestmirror
Resolving Dependencies
--> Running transaction check
---> Package sysstat.x86_64 0:5.0.5-1 will be erased
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================
 Package                    Arch                      Version                     Repository                    Size
=====================================================================================================================
Removing:
 sysstat                    x86_64                    5.0.5-1                     installed                    307 k

Transaction Summary
=====================================================================================================================
Remove  1 Package

Installed size: 307 k
Is this ok [y/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Erasing    : sysstat-5.0.5-1.x86_64                                                                            1/1
  Verifying  : sysstat-5.0.5-1.x86_64                                                                            1/1

Removed:
  sysstat.x86_64 0:5.0.5-1

Complete!
```

#### 6. Install EPEL repository and get information about it.
```bash
[sitis@localhost lesson7]$ yum search epel-release
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.axelname.ru
 * extras: mirror.axelname.ru
 * updates: mirrors.powernet.com.ru
============================================= N/S matched: epel-release =============================================
epel-release.noarch : Extra Packages for Enterprise Linux repository configuration

  Name and summary matches only, use "search all" for everything.
[sitis@localhost lesson7]$ sudo yum -y install epel-release
[sudo] password for sitis:
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos-mirror.rbc.ru
 * extras: centos-mirror.rbc.ru
 * updates: centos-mirror.rbc.ru
Resolving Dependencies
--> Running transaction check
---> Package epel-release.noarch 0:7-11 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=====================================================================================================================
 Package                         Arch                      Version                   Repository                 Size
=====================================================================================================================
Installing:
 epel-release                    noarch                    7-11                      extras                     15 k

Transaction Summary
=====================================================================================================================
Install  1 Package

Total download size: 15 k
Installed size: 24 k
Downloading packages:
epel-release-7-11.noarch.rpm                                                                  |  15 kB  00:00:00
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : epel-release-7-11.noarch                                                                          1/1
  Verifying  : epel-release-7-11.noarch                                                                          1/1

Installed:
  epel-release.noarch 0:7-11

Complete!
[sitis@localhost lesson7]$ yum repoinfo epel-release
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.sale-dedic.com
 * epel: fedora-epel.koyanet.lv
 * extras: mirror.sale-dedic.com
 * updates: mirrors.powernet.com.ru
repolist: 0
[sitis@localhost lesson7]$ yum repoinfo epel
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.sale-dedic.com
 * epel: mirror.speedpartner.de
 * extras: mirror.sale-dedic.com
 * updates: mirrors.powernet.com.ru
Repo-id      : epel/x86_64
Repo-name    : Extra Packages for Enterprise Linux 7 - x86_64
Repo-status  : enabled
Repo-revision: 1639877709
Repo-updated : Sun Dec 19 04:47:01 2021
Repo-pkgs    : 13,694
Repo-size    : 16 G
Repo-metalink: https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=x86_64
  Updated    : Sun Dec 19 04:47:01 2021
Repo-baseurl : http://mirror.speedpartner.de/epel/7/x86_64/ (68 more)
Repo-expire  : 21,600 second(s) (last: Sun Dec 19 15:23:44 2021)
  Filter     : read-only:present
Repo-filename: /etc/yum.repos.d/epel.repo

```

#### 7. Find how much packages provided exactly by EPEL repository.
```bash
[sitis@localhost lesson7]$ sudo yum repo-pkgs epel list
```

#### 8. Install ncdu package from EPEL repo.
```bash
[sitis@localhost lesson7]$ sudo yum repo-pkgs epel list | grep ncdu
ncdu.x86_64                                 1.16-1.el7                      epel
[sitis@localhost lesson7]$ yum install ncdu.x86_64
```

#### *Extra task: Need to create an rpm package consists of a shell script and a text file. The script should output words count stored in file.

### Work with files

#### 1. Find all regular files below 100 bytes inside your home directory.
```bash
[sitis@localhost lesson7]$ find ~/ -size -100c -type f
/home/sitis/.bash_logout
/home/sitis/.ssh/config
/home/sitis/scr/err.log
/home/sitis/scr/fileForDir
/home/sitis/err.log
/home/sitis/task2.txt
/home/sitis/.lesshst
/home/sitis/lesson5/simpleDaemon
/home/sitis/lesson5/oneshotDaemon
/home/sitis/lesson5/stdout
/home/sitis/lesson5/stderr
/home/sitis/script
/home/sitis/stdout
/home/sitis/stderr
```

#### 2. Find an inode number and a hard links count for the root directory. The hard link count should be about 17. Why?
```bash
[sitis@localhost lesson7]$ ls -ali /
total 16
      64 dr-xr-xr-x.  17 root root  224 Nov 25 19:08 .
      64 dr-xr-xr-x.  17 root root  224 Nov 25 19:08 ..
```
> Each directory inside adds a link when created.
```console
[sitis@localhost lesson7]$ ls -la /
total 16
dr-xr-xr-x.  17 root root  224 Nov 25 19:08 .                           1
dr-xr-xr-x.  17 root root  224 Nov 25 19:08 ..                          2
lrwxrwxrwx.   1 root root    7 Nov 25 19:05 bin -> usr/bin
dr-xr-xr-x.   5 root root 4096 Dec  4 21:11 boot                        3
drwxr-xr-x.  20 root root 3080 Dec 19 10:05 dev                         4
drwxr-xr-x.  74 root root 8192 Dec 19 14:58 etc                         5
drwxr-xr-x.   4 root root   30 Dec  2 21:16 home                        6
lrwxrwxrwx.   1 root root    7 Nov 25 19:05 lib -> usr/lib
lrwxrwxrwx.   1 root root    9 Nov 25 19:05 lib64 -> usr/lib64
drwxr-xr-x.   2 root root    6 Apr 11  2018 media                       7
drwxr-xr-x.   2 root root    6 Apr 11  2018 mnt                         8
drwxr-xr-x.   2 root root   19 Dec 11 02:06 opt                         9
dr-xr-xr-x. 107 root root    0 Dec 19 10:05 proc                        10
dr-xr-x---.   5 root root  225 Dec 19 14:03 root                        11
drwxr-xr-x.  23 root root  700 Dec 19 15:34 run                         12
lrwxrwxrwx.   1 root root    8 Nov 25 19:05 sbin -> usr/sbin
drwxr-xr-x.   2 root root    6 Apr 11  2018 srv                         13
dr-xr-xr-x.  13 root root    0 Dec 19 10:05 sys                         14
drwxrwxrwt.   9 root root  183 Dec 19 15:34 tmp                         15
drwxr-xr-x.  13 root root  155 Nov 25 19:05 usr                         16
drwxr-xr-x.  19 root root  267 Nov 25 19:11 var                         17

```

#### 3. Check what inode numbers have "/" and "/boot" directory. Why?
```bash
[sitis@localhost lesson7]$ ls -ali / | grep boot
      64 dr-xr-xr-x.   5 root root 4096 Dec  4 21:11 boot
[sitis@localhost lesson7]$ ls -ali /
total 16
      64 dr-xr-xr-x.  17 root root  224 Nov 25 19:08 .
      64 dr-xr-xr-x.  17 root root  224 Nov 25 19:08 ..
[sitis@localhost lesson7]$ df -Th
Filesystem              Type      Size  Used Avail Use% Mounted on
devtmpfs                devtmpfs  908M     0  908M   0% /dev
tmpfs                   tmpfs     919M     0  919M   0% /dev/shm
tmpfs                   tmpfs     919M  8.6M  911M   1% /run
tmpfs                   tmpfs     919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root xfs       6.2G  2.0G  4.3G  33% /
/dev/sda1               xfs      1014M  194M  821M  20% /boot
tmpfs                   tmpfs     184M     0  184M   0% /run/user/1000
```
> Since they are on different filesystems.

#### 4. Check the root directory space usage by du command. Compare it with an information from df. If you find differences, try to find out why it happens.
```bash
[sitis@localhost lesson7]$ df -h /
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  6.2G  2.0G  4.3G  33% /
[sitis@localhost lesson7]$ du -sh /
2.1G    /
```
> The difference is that du queries directly for every file found in the partition, while df queries the filesystem. When a file is deleted, which at that moment was “occupied” by a process, its name is deleted, but the inode remains in the file system until the process that “holds” this file ends.

#### 5. Check disk space usage of /var/log directory using ncdu
```bash
[sitis@localhost lesson7]$ ncdu /var/log
--- /var/log --------------------------------------------------------------------------------------------------------
    1.8 MiB [################] /anaconda
    1.3 MiB [###########     ]  messages-20211219
  984.0 KiB [########        ]  messages-20211214
  728.0 KiB [######          ]  messages-20211202
  240.0 KiB [##              ]  messages-20211205
  172.0 KiB [#               ]  secure-20211214
  148.0 KiB [#               ]  wtmp
  148.0 KiB [#               ]  boot.log-20211219
   76.0 KiB [                ] /tuned
   60.0 KiB [                ]  boot.log-20211211
   56.0 KiB [                ]  secure-20211219
   32.0 KiB [                ]  dmesg.old
   32.0 KiB [                ]  dmesg
   28.0 KiB [                ]  secure
   20.0 KiB [                ]  cron-20211214
   20.0 KiB [                ]  maillog-20211219
   20.0 KiB [                ]  boot.log-20211216
   20.0 KiB [                ]  boot.log-20211214
   16.0 KiB [                ]  lastlog
   16.0 KiB [                ]  boot.log-20211210
   16.0 KiB [                ]  boot.log-20211209
   16.0 KiB [                ]  secure-20211202
   16.0 KiB [                ]  cron-20211219
   12.0 KiB [                ]  firewalld
   12.0 KiB [                ]  yum.log
   12.0 KiB [                ]  cron-20211202
   12.0 KiB [                ]  maillog-20211214
   12.0 KiB [                ]  boot.log-20211218
    8.0 KiB [                ]  tallylog
    8.0 KiB [                ]  secure-20211205
    8.0 KiB [                ]  cron-20211205
    8.0 KiB [                ]  maillog-20211202
    8.0 KiB [                ]  messages
    4.0 KiB [                ]  cron
    4.0 KiB [                ]  btmp
    4.0 KiB [                ]  auth-errors
    4.0 KiB [                ]  grubby
    4.0 KiB [                ]  maillog-20211205
    4.0 KiB [                ]  btmp-20211202
    4.0 KiB [                ]  grubby_prune_debug
    0.0   B [                ] /nginx
!   0.0   B [                ] /audit
e   0.0   B [                ] /rhsm
    0.0   B [                ]  spooler-20211219
    0.0   B [                ]  spooler-20211214
    0.0   B [                ]  spooler-20211205
    0.0   B [                ]  spooler-20211202
    0.0   B [                ]  spooler
    0.0   B [                ]  maillog
    0.0   B [                ]  boot.log

```
