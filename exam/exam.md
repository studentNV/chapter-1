# Инструкции по выполнению.
## 1. Настройка до начала установки `CentOS`.
#### Установить `CentOS` на 2 виртуальные машины:
#### •	VM1: 2CPU, 2-4G памяти, системный диск на 15-20G и дополнительные 2 диска по 5G
#### •	VM2: 2CPU, 2-4G памяти, системный диск на 15-20G и дополнительные 2 диска по 5G
> При создании виртуальных машин добавляем отдельно два диска по `5 Gb`.

![image](https://user-images.githubusercontent.com/95025513/150646542-b368bdc6-6c7f-4b22-855c-7cd512e72f7f.png)
> Так же каждой виртуальной машине выделяем по два ядра.

![image](https://user-images.githubusercontent.com/95025513/150646564-0e6653ef-ba64-4df5-bcf1-593c7a7353be.png)
## 2. Установка `CentOS` и создание пользователя.
#### При установке `CentOS` создать дополнительного пользователя exam и настроить для него использование `sudo` без пароля. Все последующие действия необходимо выполнять от этого пользователя, если не указано иное.
> Первым делом устанавливаем язык для системы наш выбор `English`.

![image](https://user-images.githubusercontent.com/95025513/150646588-db35c8ab-c3a3-486e-bd8e-8f74db884146.png)
> Далее выбираем часовой пояс.

![image](https://user-images.githubusercontent.com/95025513/150646601-6a6c1ef8-1c0c-4ee2-ac23-857a85fbb87a.png)
> Затем оставляем автоматическую настройку нашего основного хранилища на `20 Gb`. Дополнительные два диска оставляем нетронутыми для дальнейших манипуляций по заданию.

![image](https://user-images.githubusercontent.com/95025513/150646626-c60134c5-24ec-49ac-ad09-525076492c94.png)
> Не забываем включить наш сетевой адаптер в соответствующей вкладке.

![image](https://user-images.githubusercontent.com/95025513/150646647-22b3ca85-6074-47d3-b29a-1573fafd2d23.png)
> Во время установки системы задаем пароль пользователю `root` и создаем своего много почтенного пользователя `exam`.

![image](https://user-images.githubusercontent.com/95025513/150646662-50f450fd-c4f0-4ad8-8030-acd683cbe9ec.png)
> После установки системы изменяем сетевые настройки в `Oracle Virtual Box` на `Сетевой мост`.

![image](https://user-images.githubusercontent.com/95025513/150646674-07ded84c-2cd5-40cd-afd0-d8c826a7fd18.png)
> На самих виртуальных машинах задаём статический ip-адрес:
> - vm1-headnode	192.168.100.11
> - vm2-worker 		192.168.100.12
> 
> После этого производим перезагрузку системы.
> Далее для того что бы каждый раз не вводить пароль при использовании `sudo` произведём следующие манипуляции. С помощью команды `sudo visudo` редактируем файл `/etc/sudoers` добавляя строчку. 
```bash
exam    ALL=(ALL)       NOPASSWD: ALL
```
> Так же перед выполнением дальнейших действий обновляем пакеты с помощью команды `sudo yum upgrade` на обеих машинах.
## 3. Установка `OpenJDK8`
#### Установить `OpenJDK8` из репозитория `CentOS`.
> Заходим на официальный сайт `OpenJDK https://openjdk.java.net/install/` и видим команду для актуального скачивания и установки программы выполняем её.
```bash
[exam@vm1-headnode ~]$ sudo yum install java-1.8.0-openjdk –y
```
## 4. Скачивание архива с Hadoop
#### Скачать архив с Hadoop версии 3.1.2 (https://hadoop.apache.org/release/3.1.2.html)
> Для удобства создаём папку и уже в неё скачиваю архив, так же перед этим скачиваем и устанавливаем утилиту `wget`.
```bash
[exam@vm1-headnode ~]$ sudo yum install wget -y
[exam@vm1-headnode ~]$ mkdir point4
[exam@vm1-headnode ~]$ cd point4/
[exam@vm1-headnode point4]$ wget https://archive.apache.org/dist/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
[exam@vm1-headnode point4]$ ll
total 324644
-rw-rw-r--. 1 exam exam 332433589 Feb  6  2019 hadoop-3.1.2.tar.gz
```
## 5. Распаковка содержимого архива
#### Распаковать содержимое архива в `/opt/hadoop-3.1.2/`
> Перед тем как распаковать архив создаем директорию `hadoop-3.1.2` в `opt`.
```bash
[exam@vm1-headnode point4]$ sudo mkdir /opt/hadoop-3.1.2/
[exam@vm1-headnode point4]$ sudo tar -xvf  hadoop-3.1.2.tar.gz -C /opt/hadoop-3.1.2/
[exam@vm1-headnode point4]$ ll /opt/hadoop-3.1.2/
total 0
drwxr-xr-x. 9 1001 1002 149 Jan 29  2019 hadoop-3.1.2
```
## 6. Создание символьной ссылки
#### Сделать симлинк `/usr/local/hadoop/current/` на директорию `/opt/hadoop-3.1.2/`
> Создаем симлинк следующей командой.
```bash
[exam@vm1-headnode ~]$ sudo ln -s /opt/hadoop-3.1.2/* /usr/local/hadoop/current/
[exam@vm1-headnode ~]$ sudo ls -la /usr/local/hadoop/current
total 0
drwxr-xr-x. 2 root root 26 Jan 17 19:28 .
drwxr-xr-x. 3 root root 21 Jan 15 18:54 ..
lrwxrwxrwx. 1 root root 30 Jan 17 19:27 hadoop-3.1.2 -> /opt/hadoop-3.1.2/hadoop-3.1.2
```
## 7. Создание пользователей
#### Создать пользователей `hadoop`, `yarn` и `hdfs`, а также группу `hadoop`, в которую необходимо добавить всех этих пользователей
> Создаем пользователей `рadoop`, `yarn` и `hdfs`
```bash
[exam@vm1-headnode point4]$ sudo useradd hadoop
[exam@vm1-headnode point4]$ sudo useradd yarn
[exam@vm1-headnode point4]$ sudo useradd hdfs
```
> Добавляем всех созданных пользователей в группу `hadoop`.
```bash
[exam@vm1-headnode point4]$ getent group | grep hadoop
hadoop:x:1001:
[exam@vm1-headnode point4]$ sudo usermod -a -G hadoop hdfs
[exam@vm1-headnode point4]$ sudo usermod -a -G hadoop yarn
```
> Проверяем каждого пользователя.
```bash
[exam@vm1-headnode point4]$ sudo groups hdfs
hdfs : hdfs hadoop
[exam@vm1-headnode point4]$ sudo groups hadoop
hadoop : hadoop
[exam@vm1-headnode point4]$ sudo groups yarn
yarn : yarn Hadoop
```
## 8. Создание разделов для дисков
#### Создать для обоих дополнительных дисков разделы размером в `100%` диска.
> Для начала посмотрим названия устройств.
```bash
[exam@vm1-headnode point4]$ lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0   19G  0 part
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
sdc               8:32   0    5G  0 disk
sr0              11:0    1 1024M  0 rom 
```
> Теперь зная названия, можем создавать на первом устройстве.
```bash
[exam@vm1-headnode point4]$ sudo fdisk /dev/sdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xf03cccee.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p):
Using default response p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
Partition 1 of type Linux and of size 5 GiB is set

Command (m for help): p

Disk /dev/sdb: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xf03cccee

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
> После этого можно заняться и вторым.
```
[exam@vm1-headnode point4]$ sudo fdisk /dev/sdc
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xd467249d.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p):
Using default response p
Partition number (1-4, default 1):
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
Partition 1 of type Linux and of size 5 GiB is set

Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xd467249d

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
> Смотрим результат проделанной работы.
```bash
[exam@vm1-headnode point4]$ lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda               8:0    0   20G  0 disk
├─sda1            8:1    0    1G  0 part /boot
└─sda2            8:2    0   19G  0 part
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
sdb               8:16   0    5G  0 disk
└─sdb1            8:17   0    5G  0 part
sdc               8:32   0    5G  0 disk
└─sdc1            8:33   0    5G  0 part
sr0              11:0    1 1024M  0 rom 
```
## 9. Инициализация разделов для `LVM`
#### Инициализировать разделы из `п.8` в качестве физических томов для `LVM`.
> Для начала отобразим физические тома.
```bash
 [exam@vm1-headnode point4]$ sudo pvscan
  PV /dev/sda2   VG centos          lvm2 [<19.00 GiB / 0    free]
  Total: 1 [<19.00 GiB] / in use: 1 [<19.00 GiB] / in no VG: 0 [0   ]
```
> После инициализируем нужные нам.
```bash
[exam@vm1-headnode point4]$ sudo pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
[exam@vm1-headnode point4]$ sudo pvcreate /dev/sdc1
  Physical volume "/dev/sdc1" successfully created.
```
> Теперь можем сравнить результат.
```bash
[exam@vm1-headnode ~]$ sudo pvscan
  PV /dev/sda2   VG centos          lvm2 [<19.00 GiB / 0    free]
  PV /dev/sdb1                      lvm2 [<5.00 GiB]
  PV /dev/sdc1                      lvm2 [<5.00 GiB]
  Total: 3 [28.99 GiB] / in use: 1 [<19.00 GiB] / in no VG: 2 [<10.00 GiB]
```
## 10. Создание двух групп `LVM`
#### Создать две группы `LVM` и добавить в каждую из них по одному физическому тому из `п.9`.
> Отобразим физические тома и обратим внимание на строку с `VG Name`.
```bash
[exam@vm1-headnode point4]$ sudo pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               <19.00 GiB / not usable 3.00 MiB
  Allocatable           yes (but full)
  PE Size               4.00 MiB
  Total PE              4863
  Free PE               0
  Allocated PE          4863
  PV UUID               CbBnUd-nzUh-4O1w-wQ99-D8pi-BSec-timSLS

  "/dev/sdb1" is a new physical volume of "<5.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name
  PV Size               <5.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               Bhhxvx-aZwE-6WN9-MCTd-Dz0G-onNu-W7QPIq

  "/dev/sdc1" is a new physical volume of "<5.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdc1
  VG Name
  PV Size               <5.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               ocNRHX-WSr6-T50P-8ijN-M5w8-xv3V-OrE8dy
```
> Создаем `LVM` тома и добавляем в них наши физические.
```bash
[exam@vm1-headnode point4]$ sudo vgcreate vol_grp1 /dev/sdb1
  Volume group "vol_grp1" successfully created
[exam@vm1-headnode point4]$ sudo vgcreate vol_grp2 /dev/sdc1
  Volume group "vol_grp2" successfully created
```
> Опять отображаем информацию о томах и так же смотрим на строку `VG Name`.
```bash
[exam@vm1-headnode point4]$ sudo pvdisplay
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               centos
  PV Size               <207.00 GiB / not usable 3.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              52991
  Free PE               1
  Allocated PE          52990
  PV UUID               2zU7aK-pb3T-Pdyf-G7J5-ZvGc-gKzv-ogwAs8

  --- Physical volume ---
  PV Name               /dev/sdc1
  VG Name               vol_grp2
  PV Size               <5.00 GiB / not usable 3.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              1279
  Free PE               1279
  Allocated PE          0
  PV UUID               DKtMsc-aj32-Sn63-ik2I-sdop-QnHR-CfvAWF

  --- Physical volume ---
  PV Name               /dev/sdb1
  VG Name               vol_grp1
  PV Size               <5.00 GiB / not usable 3.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              1279
  Free PE               1279
  Allocated PE          0
  PV UUID               aFCZfU-0IEU-QQM7-xmOW-yMol-7nRP-3afDgJ
```
## 11. Создание логических томов в `LVM`.
#### В каждой из групп из `п.10` создать логический том `LVM` размером `100%` группы.
> Перед созданием посмотрим что имеем на данный момент.
```bash
 [exam@vm1-headnode ~]$ sudo lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao---- <17.00g
  swap centos -wi-ao----   2.00g 
```
> Создаем логические тома.
```bash
[exam@vm1-headnode ~]$ sudo lvcreate -l 100%FREE -n logical_vol1 vol_grp1
  Logical volume "logical_vol1" created.
[exam@vm1-headnode ~]$ sudo lvcreate -l 100%FREE -n logical_vol2 vol_grp2
  Logical volume "logical_vol2" created.
```
> Смотрим на результат создание логических томов.
```bash
[exam@vm1-headnode ~]$ sudo lvs
  LV           VG       Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root         centos   -wi-ao---- <17.00g
  swap         centos   -wi-ao----   2.00g
  logical_vol1 vol_grp1 -wi-a-----  <5.00g
  logical_vol2 vol_grp2 -wi-a-----  <5.00g
```
## 12. Создание файловой системы `ext4`.
#### На каждом логическом томе `LVM` создать файловую систему `ext4`.
> Сначала посмотрим информацию о файловых системах.
```bash
[exam@vm1-headnode ~]$ lsblk -f
NAME                      FSTYPE      LABEL UUID                                   MOUNTPOINT
sda
├─sda1                    xfs               d02fbc89-f394-44d5-ab83-e007e8ae903e   /boot
└─sda2                    LVM2_member       2zU7aK-pb3T-Pdyf-G7J5-ZvGc-gKzv-ogwAs8
  ├─centos-root           xfs               4417fa68-bbbc-424e-907a-1f24cf293f43   /
  ├─centos-swap           swap              fa1c8527-0a5e-4ca0-8ff4-f904d31a1015   [SWAP]
  └─centos-home           xfs               c91f2489-0b68-45dc-902d-86b5e36be4dd   /home
sdb
└─sdb1                    LVM2_member       aFCZfU-0IEU-QQM7-xmOW-yMol-7nRP-3afDgJ
  └─vol_grp1-logical_vol1
sdc
└─sdc1                    LVM2_member       DKtMsc-aj32-Sn63-ik2I-sdop-QnHR-CfvAWF
  └─vol_grp2-logical_vol2
sr0
```
> Затем создадим файловые системы.
```bash
[exam@vm1-headnode ~]$ sudo mkfs.ext4 /dev/vol_grp1/logical_vol1
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1309696 blocks
65484 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

[exam@vm1-headnode ~]$ sudo mkfs.ext4 /dev/vol_grp2/logical_vol2
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1309696 blocks
65484 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```
> После всех манипуляций посмотрим, что изменилось.
```bash
[exam@vm1-headnode ~]$ lsblk -f
NAME                      FSTYPE      LABEL UUID                                   MOUNTPOINT
sda
├─sda1                    xfs               d02fbc89-f394-44d5-ab83-e007e8ae903e   /boot
└─sda2                    LVM2_member       2zU7aK-pb3T-Pdyf-G7J5-ZvGc-gKzv-ogwAs8
  ├─centos-root           xfs               4417fa68-bbbc-424e-907a-1f24cf293f43   /
  ├─centos-swap           swap              fa1c8527-0a5e-4ca0-8ff4-f904d31a1015   [SWAP]
  └─centos-home           xfs               c91f2489-0b68-45dc-902d-86b5e36be4dd   /home
sdb
└─sdb1                    LVM2_member       aFCZfU-0IEU-QQM7-xmOW-yMol-7nRP-3afDgJ
  └─vol_grp1-logical_vol1 ext4              3c93c6f4-f9d0-47e3-b3a3-597e4ffae881
sdc
└─sdc1                    LVM2_member       DKtMsc-aj32-Sn63-ik2I-sdop-QnHR-CfvAWF
  └─vol_grp2-logical_vol2 ext4              ac0ea0ca-cd3f-4d50-a9f1-f44c7b4e97e0
sr0
```
> На наших логических томах создалась файловая система `ext4`.
## 13. Создание директорий для точек минирования файловых систем.
#### Создать директории и использовать их в качестве точек монтирования файловых систем из `п.12`:
#### •	/opt/mount1
#### •	/opt/mount2
> Создаем директории и смотрим на результат.
```bash
[exam@vm1-headnode ~]$ sudo mkdir /opt/mount1 /opt/mount2
[exam@vm1-headnode ~]$ ll /opt/
total 0
drwxr-xr-x. 3 root root 26 Jan 15 14:39 hadoop-3.1.2
drwxr-xr-x. 2 root root  6 Jan 15 17:28 mount1
drwxr-xr-x. 2 root root  6 Jan 15 17:28 mount2
```
## 14. Монтирование систем.
#### Настроить систему так, чтобы монтирование происходило автоматически при запуске системы. Произвести монтирование новых файловых систем.
> Перед началом монтирования посмотрим информацию о уже смонтированных системах.
```bash
[exam@vm1-headnode ~]$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                         8:0    0   208G  0 disk
├─sda1                      8:1    0     1G  0 part /boot
└─sda2                      8:2    0   207G  0 part
  ├─centos-root           253:0    0    50G  0 lvm  /
  ├─centos-swap           253:1    0   3.9G  0 lvm  [SWAP]
  └─centos-home           253:2    0 153.1G  0 lvm  /home
sdb                         8:16   0     5G  0 disk
└─sdb1                      8:17   0     5G  0 part
  └─vol_grp1-logical_vol1 253:3    0     5G  0 lvm
sdc                         8:32   0     5G  0 disk
└─sdc1                      8:33   0     5G  0 part
  └─vol_grp2-logical_vol2 253:4    0     5G  0 lvm
sr0                        11:0    1  1024M  0 rom
```
> Теперь монтируем наши системы.
```bash
[exam@vm1-headnode ~]$ sudo mount /dev/vol_grp1/logical_vol1 /opt/mount1
[exam@vm1-headnode ~]$ sudo mount /dev/vol_grp2/logical_vol2 /opt/mount2
```
> После монтирования смотри что изменилось.
```bash
[exam@vm1-headnode ~]$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                         8:0    0   208G  0 disk
├─sda1                      8:1    0     1G  0 part /boot
└─sda2                      8:2    0   207G  0 part
  ├─centos-root           253:0    0    50G  0 lvm  /
  ├─centos-swap           253:1    0   3.9G  0 lvm  [SWAP]
  └─centos-home           253:2    0 153.1G  0 lvm  /home
sdb                         8:16   0     5G  0 disk
└─sdb1                      8:17   0     5G  0 part
  └─vol_grp1-logical_vol1 253:3    0     5G  0 lvm  /opt/mount1
sdc                         8:32   0     5G  0 disk
└─sdc1                      8:33   0     5G  0 part
  └─vol_grp2-logical_vol2 253:4    0     5G  0 lvm  /opt/mount2
sr0                        11:0    1  1024M  0 rom
```
> После всех манипуляций наши системы смонтированных корректно, но после перезагрузки изменения пропадут давайте исправим это. Для этого в файл `/etc/fstab` добавляем следующие 2 строчки. Но перед этим посмотрим на их `UUI` что бы при удалении дисков не приходилось каждый раз править этот файл.
```bash
[exam@vm1-headnode ~]$ sudo blkid
/dev/sda1: UUID="5b0fccd1-eb0d-49c7-95be-477b59cb6a05" TYPE="xfs"
/dev/sda2: UUID="XIk7qD-oq0f-cGh0-OoPf-oifA-NIYd-eGXtoi" TYPE="LVM2_member"
/dev/sdc1: UUID="RO9Ou0-YWlG-odYz-gwyh-8Bsx-7lkz-fyoWWz" TYPE="LVM2_member"
/dev/sdb1: UUID="K8eJnm-NNlV-FhQA-BPI3-5nW9-LeqU-tX2Y7V" TYPE="LVM2_member"
/dev/mapper/centos-root: UUID="efa1e9aa-a1c9-400c-b795-9ee550a984de" TYPE="xfs"
/dev/mapper/centos-swap: UUID="c6aa38d4-d6c5-407b-82a0-1fd94c1cf6cd" TYPE="swap"
/dev/mapper/vol_grp1-logical_vol1: UUID="05a7a4d8-e154-4641-a9b9-ff600048bc9a" TYPE="ext4"
/dev/mapper/vol_grp2-logical_vol2: UUID="244d52fd-2f58-42d8-842a-0ff1ce7e943e" TYPE="ext4"
Теперь можно редактировать файл «/etc/fstab».
UUID="05a7a4d8-e154-4641-a9b9-ff600048bc9a"     /opt/mount1     ext4    defaults        0 0
UUID="244d52fd-2f58-42d8-842a-0ff1ce7e943e"      /opt/mount2     ext4    defaults        0 0
```
> И проверяем результат.
```bash
[exam@vm1-headnode ~]$ reboot
~~~~~~~~~~~~~~~~~
After some time.
~~~~~~~~~~~~~~~~~
[exam@vm1-headnode ~]$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                         8:0    0   208G  0 disk
├─sda1                      8:1    0     1G  0 part /boot
└─sda2                      8:2    0   207G  0 part
  ├─centos-root           253:0    0    50G  0 lvm  /
  ├─centos-swap           253:1    0   3.9G  0 lvm  [SWAP]
  └─centos-home           253:2    0 153.1G  0 lvm  /home
sdb                         8:16   0     5G  0 disk
└─sdb1                      8:17   0     5G  0 part
  └─vol_grp1-logical_vol1 253:3    0     5G  0 lvm  /opt/mount1
sdc                         8:32   0     5G  0 disk
└─sdc1                      8:33   0     5G  0 part
  └─vol_grp2-logical_vol2 253:4    0     5G  0 lvm  /opt/mount2
sr0                        11:0    1  1024M  0 rom
```
> Монтирование не слетело.

```bash
[exam@vm1-headnode ~]$ df -h /opt/mount1
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/vol_grp1-logical_vol1  4.8G   20M  4.6G   1% /opt/mount1
[exam@vm1-headnode ~]$ df -h /opt/mount2
Filesystem                         Size  Used Avail Use% Mounted on
/dev/mapper/vol_grp2-logical_vol2  4.8G   20M  4.6G   1% /opt/mount2
```
# Для `VM1` (шаги 15-16):
## 15. Создание директорий для файлов `Namenode`.
### После монтирования создать 2 директории для хранения файлов Namenode сервиса `HDFS`:
### •	/opt/mount1/namenode-dir
### •	/opt/mount2/namenode-dir
> Смотрим что сейчас находиться в этих директориях.
```bash
[exam@vm1-headnode ~]$ ls /opt/mount1 /opt/mount2
/opt/mount1:
lost+found

/opt/mount2:
lost+found
```
> Только созданные системой две директории. Создаем наши.
```bash
[exam@vm1-headnode ~]$ sudo mkdir /opt/mount1/namenode-dir /opt/mount2/namenode-dir
```
> Проверяем наличие созданных директорий.
```bash
[exam@vm1-headnode ~]$ ls /opt/mount1 /opt/mount2
/opt/mount1:
lost+found  namenode-dir

/opt/mount2:
lost+found  namenode-dir
```
> По поводу директорий `lost+found` нашёл данную информацию в сети.
> Директория `lost+found` находится в корне файловой системы.
> Данную директорию использует утилита `fsck`. Утилита `fsck` предназначена для проверки файловой системы.
> Если утилита `fsck` в ходе проверки находит данные в файловой системе, которые повреждены или не имеют имени в системе («осиротевшие»), то такие файлы помещаются в директорию `lost+found`.
> Например, если во время записи какого-либо файла на жесткий диск, вы внезапно выключите компьютер (например, выключите питание), то `fsck` сможет потом найти данный файл и поместит его в `lost+found`.
## 16. Присвоение прав директориям для пользователя `hdfs` и группе `hadoop` на `VM1`.
#### Сделать пользователя `hdfs` и группу `hadoop` владельцами этих директорий.
> Перед присвоением посмотрим, как сейчас обстоят дела.
```bash
 [exam@vm1-headnode ~]$ ls -ls /opt/mount1 /opt/mount2
/opt/mount1:
total 20
16 drwx------. 2 root root   16384 Jan 15 19:01 lost+found
 4 drwxr-xr-x. 2 root root   4096 Jan 16 13:24 namenode-dir

/opt/mount2:
total 20
16 drwx------. 2 root root 16384 Jan 15 19:01 lost+found
 4 drwxr-xr-x. 2 root root  4096 Jan 16 13:24 namenode-dir
```
> Теперь можем присваивать права директориям, созданным в `15` пункте.
```bash
[exam@vm1-headnode ~]$ sudo chown hdfs:hadoop /opt/mount1/namenode-dir /opt/mount2/namenode-dir 
```
> А вот теперь смотрим на изменения.
```bash
[exam@vm1-headnode ~]$ ls -ls /opt/mount1 /opt/mount2
/opt/mount1:
total 20
16 drwx------. 2 root root   16384 Jan 15 19:01 lost+found
 4 drwxr-xr-x. 3 hdfs hadoop  4096 Jan 17 19:36 namenode-dir

/opt/mount2:
total 20
16 drwx------. 2 root root   16384 Jan 15 19:01 lost+found
 4 drwxr-xr-x. 3 hdfs hadoop  4096 Jan 17 19:36 namenode-dir
```
# Для `VM2` (шаги 17-20):
## 17. Создание директорий для файлов `Datanode`.
#### После монтирования создать 2 директории для хранения файлов `Datanode` сервиса `HDFS`:
#### • 	/opt/mount1/datanode-dir
#### • 	/opt/mount2/datanode-dir
> Смотрим что сейчас находиться в этих директориях.
```bash
[exam@vm2-worker ~]$ ls /opt/mount1 /opt/mount2
/opt/mount1:
lost+found

/opt/mount2:
lost+found
[exam@vm2-worker ~]$ sudo mkdir /opt/mount1/datanode-dir /opt/mount2/datanode-dir
[exam@vm2-worker ~]$ ls /opt/mount1 /opt/mount2
/opt/mount1:
datanode-dir  lost+found

/opt/mount2:
datanode-dir  lost+found
[exam@vm2-worker ~]$
```
## 18. Присвоение прав директориям для пользователя `hdfs` и группе `hadoop` на `VM2`.
#### Сделать пользователя `hdfs` и группу `hadoop` владельцами директорий из `п.17`.
> Перед присваением прав посмотрим, как сейчас обстоят дела.
```bash
[exam@vm2-worker ~]$ ls -l /opt/mount1 /opt/mount2
/opt/mount1:
total 20
drwxr-xr-x. 2 root root  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root 16384 Jan 15 18:21 lost+found

/opt/mount2:
total 20
drwxr-xr-x. 2 root root  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root 16384 Jan 15 18:21 lost+found
```
> Теперь можем присваивать права директориям, созданным в `17` пункте.
```bash
 [exam@vm2-worker ~]$ sudo chown hdfs:hadoop /opt/mount1/datanode-dir/ /opt/mount2/datanode-dir/
А вот теперь смотрим на изменения.
[exam@vm2-worker ~]$ ls -l /opt/mount1 /opt/mount2
/opt/mount1:
total 28
drwx------. 3 hdfs hadoop  4096 Jan 17 19:31 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found

/opt/mount2:
total 28
drwx------. 3 hdfs hadoop  4096 Jan 17 19:31 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
```
## 19. Создание директорий для `Nodemanager`
#### Создать дополнительные 4 директории для `Nodemanager` сервиса `YARN`:
#### •	/opt/mount1/nodemanager-local-dir
#### •	/opt/mount2/nodemanager-local-dir
#### •	/opt/mount1/nodemanager-log-dir
#### •	/opt/mount2/nodemanager-log-dir
> Период создание посмотрим, как у нас в данных директориях обстоят дела.
```bash
[exam@vm2-worker ~]$ ls -l /opt/mount1 /opt/mount1
/opt/mount1:
total 20
drwxr-xr-x. 2 hdfs hadoop  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found

/opt/mount1:
total 20
drwxr-xr-x. 2 hdfs hadoop  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
```
> Теперь можно и создавать.
```bash
[exam@vm2-worker ~]$ sudo mkdir /opt/mount1/nodemanager-local-dir /opt/mount2/nodemanager-local-dir /opt/mount1/nodemanager-log-dir /opt/mount2/nodemanager-log-dir
```
> Посмотрим правильно создались ли директории.
```bash
[exam@vm2-worker ~]$ ls -l /opt/mount1 /opt/mount1
/opt/mount1:
total 28
drwxr-xr-x. 2 hdfs hadoop  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-local-dir
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-log-dir

/opt/mount1:
total 28
drwxr-xr-x. 2 hdfs hadoop  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-local-dir
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-log-dir
```
## 20. Присвоение прав директориям для пользователя `yarn` и группе `hadoop` на `VM2`.
#### Сделать пользователя `yarn` и группу `hadoop` владельцами директорий из `п.19`.
> Смотрим какие права присвоены данным директориям.
```bash
[exam@vm2-worker ~]$ ls -l /opt/mount1 /opt/mount1
/opt/mount1:
total 28
drwxr-xr-x. 2 hdfs hadoop  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-local-dir
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-log-dir

/opt/mount1:
total 28
drwxr-xr-x. 2 hdfs hadoop  4096 Jan 16 13:45 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-local-dir
drwxr-xr-x. 2 root root    4096 Jan 16 13:54 nodemanager-log-dir
```
> Присваиваем права директориям, созданным в `19` пункте.
```bash
[exam@vm2-worker ~]$ sudo chown yarn:hadoop /opt/mount1/nodemanager-local-dir /opt/mount2/nodemanager-local-dir /opt/mount1/nodemanager-log-dir /opt/mount2/nodemanager-log-dir
```
> Смотрим на результат выполненных команд.
```bash
[exam@vm2-worker ~]$ ls -l /opt/mount1 /opt/mount1
/opt/mount1:
total 28
drwx------. 3 hdfs hadoop  4096 Jan 17 19:31 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
drwxr-xr-x. 5 yarn hadoop  4096 Jan 17 19:35 nodemanager-local-dir
drwxr-xr-x. 2 yarn hadoop  4096 Jan 17 19:35 nodemanager-log-dir

/opt/mount1:
total 28
drwx------. 3 hdfs hadoop  4096 Jan 17 19:31 datanode-dir
drwx------. 2 root root   16384 Jan 15 18:21 lost+found
drwxr-xr-x. 5 yarn hadoop  4096 Jan 17 19:35 nodemanager-local-dir
drwxr-xr-x. 2 yarn hadoop  4096 Jan 17 19:35 nodemanager-log-dir
```
# Для обеих машин `VM1` и `VM2`:
## 21. Настроить доступ по `SSH` для пользователя `hadoop` с ключом
#### Настроить доступ по `SSH`, используя ключи для пользователя `hadoop`.
> Начнем с настройки ПК с которого будем подключаться и на котором будет лежать наш приватный ключ у меня это `Ubuntu` на ней необходимо произвести пару настроек, во-первых, выбрать в настройках `Oracle Virtual Box` сетевой мост, как и на всех остальных виртуальных машинах.

![image](https://user-images.githubusercontent.com/95025513/150647670-ea27ec0d-2a73-468a-8521-cc3502ad6ef2.png)
> Помимо этого, создаём нашу пару ключей.
```bash
sitis@sitis-VirtualBox:~$ sudo ssh-keygen -f ~/.ssh/task21
```
> Теперь на наших двух хостах необходимо задать пароль пользователю `hadoop` что бы с помощью команды `ssh-copy-id` передать наш публичный ключ на хосты.
```bash
[exam@vm1-headnode ~]$ sudo passwd hadoop
Changing password for user hadoop.
New password:
BAD PASSWORD: The password is shorter than 8 characters
Retype new password:
passwd: all authentication tokens updated successfully.
```
> Теперь на `Ubuntu` перекидываем ключ на оба хоста.
```bash
sitis@sitis-VirtualBox:~$ sudo ssh -i /home/sitis/.ssh/task21 hadoop@192.168.100.11
sitis@sitis-VirtualBox:~$ sudo ssh -i /home/sitis/.ssh/task21 hadoop@192.168.100.12
```
> Пробуем подключиться.
```bash
sitis@sitis-VirtualBox:~$ sudo ssh -i /home/sitis/.ssh/task21 hadoop@192.168.100.12
Last login: Sun Jan 16 18:31:27 2022 from 192.168.100.9
[hadoop@vm2-worker ~]$
```
> Всё работает.
## 22. Добавление хостов в файл `hosts`
#### Добавить `VM1` и `VM2` в `/etc/hosts`.
> Открываем файл `hosts` на `VM1` и добавляем следующую строчку.
```bash
192.168.100.12  vm2-worker
```
> Открываем файл `hosts` на `VM2` и добавляем следующую строчку.
```bash
192.168.100.11  vm1-headnode
```
> Проверим результат сначала на `VM1`.
```bash
[exam@vm1-headnode ~]$ ping vm2-worker
PING vm2-worker (192.168.100.12) 56(84) bytes of data.
64 bytes from vm2-worker (192.168.100.12): icmp_seq=1 ttl=64 time=0.299 ms
64 bytes from vm2-worker (192.168.100.12): icmp_seq=2 ttl=64 time=0.941 ms
^C
--- vm2-worker ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.299/0.620/0.941/0.321 ms
```
Затем на `VM2`.
```bash
[exam@vm2-worker ~]$ ping vm1-headnode
PING vm1-headnode (192.168.100.11) 56(84) bytes of data.
64 bytes from vm1-headnode (192.168.100.11): icmp_seq=1 ttl=64 time=0.288 ms
^C
--- vm1-headnode ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.288/0.288/0.288/0.000 ms
```
## 23. Скачивание и редактирования файлов для Hadoop.
#### Скачать файлы по ссылкам в /usr/local/hadoop/current/etc/hadoop/{hadoop-env.sh,core-site.xml,hdfs-site.xml,yarn-site.xml}. При помощи sed заменить заглушки на необходимые значения 
#### •	hadoop-env.sh (https://gist.github.com/rdaadr/2f42f248f02aeda18105805493bb0e9b)
#### Необходимо определить переменные JAVA_HOME (путь до директории с OpenJDK8, установленную в п.3), HADOOP_HOME (необходимо указать путь к симлинку из п.6) и HADOOP_HEAPSIZE_MAX (укажите значение в 512M)
#### •	core-site.xml (https://gist.github.com/rdaadr/64b9abd1700e15f04147ea48bc72b3c7)
#### Необходимо указать имя хоста, на котором будет запущена HDFS Namenode (VM1)
#### •	hdfs-site.xml (https://gist.github.com/rdaadr/2bedf24fd2721bad276e416b57d63e38)
#### Необходимо указать директории namenode-dir, а также datanode-dir, каждый раз через запятую (например, /opt/mount1/namenode-dir,/opt/mount2/namenode-dir)
#### •	yarn-site.xml (https://gist.github.com/Stupnikov-NA/ba87c0072cd51aa85c9ee6334cc99158)
#### Необходимо подставить имя хоста, на котором будет развернут YARN Resource Manager (VM1), а также пути до директорий nodemanager-local-dir и nodemanager-log-dir (если необходимо указать несколько директорий, то необходимо их разделить запятыми)
> Для удобства скачиваем файл в созданную папку.
```bash
[exam@vm1-headnode ~]$ mkdir point23
[exam@vm1-headnode point23]$ wget https://gist.githubusercontent.com/rdaadr/2f42f248f02aeda18105805493bb0e9b/raw/6303e424373b3459bcf3720b253c01373666fe7c/hadoop-env.sh
--2022-01-16 19:12:36--  https://gist.githubusercontent.com/rdaadr/2f42f248f02aeda18105805493bb0e9b/raw/6303e424373b3459bcf3720b253c01373666fe7c/hadoop-env.sh
Resolving gist.githubusercontent.com (gist.githubusercontent.com)... 185.199.109.133, 185.199.108.133, 185.199.110.133, ...
Connecting to gist.githubusercontent.com (gist.githubusercontent.com)|185.199.109.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15980 (16K) [text/plain]
Saving to: ‘hadoop-env.sh’

100%[===========================================================================>] 15,980      --.-K/s   in 0.009s

2022-01-16 19:12:37 (1.69 MB/s) - ‘hadoop-env.sh’ saved [15980/15980]
```
> Далее производим изменения с помощью утилиты `sed`.
```bash
[exam@vm1-headnode point23]$ sed -i 's|"%PATH_TO_OPENJDK8_INSTALLATION%"|/usr/lib/jvm/jre-1.8.0-openjdk|' hadoop-env.sh
[exam@vm1-headnode point23]$ sed -i 's|"%PATH_TO_HADOOP_INSTALLATION"|/usr/local/hadoop/current/hadoop-3.1.2|' hadoop-env.sh
[exam@vm1-headnode point23]$ sed -i 's|"%HADOOP_HEAP_SIZE%"|512M|' hadoop-env.sh
```
> Проверяем правильно мы все изменили.
```bash
[exam@vm1-headnode hadoop]$ grep -v '^$\|^#' hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/jre-1.8.0-openjdk 
export HADOOP_HOME=/usr/local/hadoop/current/hadoop-3.1.2
export HADOOP_HEAPSIZE_MAX=512M
export HADOOP_OS_TYPE=${HADOOP_OS_TYPE:-$(uname -s)}
```
> Следующий файл `core-site.xml` скачиваем и изменяем.
```bash
[exam@vm1-headnode point23]$ wget https://gist.githubusercontent.com/rdaadr/64b9abd1700e15f04147ea48bc72b3c7/raw/2d416bf137cba81b107508153621ee548e2c877d/core-site.xml
[exam@vm1-headnode point23]$ sed -i 's|%HDFS_NAMENODE_HOSTNAME%|vm1-headnode|' core-site.xml
```
> Проверяем результат.
```bash
[exam@vm1-headnode point23]$ tail -3 core-site.xml
        <value>hdfs://vm1-headnode:8020</value>
    </property>
</configuration>
```
> Следующий файл `hdfs-site.xml` скачиваем и изменяем.
```bash
[exam@vm1-headnode point23]$ wget https://gist.githubusercontent.com/rdaadr/2bedf24fd2721bad276e416b57d63e38/raw/640ee95adafa31a70869b54767104b826964af48/hdfs-site.xml
[exam@vm1-headnode point23]$ sed -i 's|%NAMENODE_DIRS%|/opt/mount1/namenode-dir,/opt/mount2/namenode-dir|'  hdfs-site.xml
[exam@vm1-headnode point23]$ sed -i 's|%DATANODE_DIRS%|/opt/mount1/datanode-dir,/opt/mount2/datanode-dir|'  hdfs-site.xml
```
> Проверяем результат.
```bash
[exam@vm1-headnode point23]$ cat hdfs-site.xml |grep " <value>"
      <value>/opt/mount1/namenode-dir,/opt/mount2/namenode-dir</value>
      <value>/opt/mount1/datanode-dir,/opt/mount2/datanode-dir</value>
      <value>1</value>
```
> Следующий файл `yarn-site.xml` скачиваем и изменяем.
```bash
[exam@vm1-headnode point23]$ wget https://gist.githubusercontent.com/Stupnikov-NA/ba87c0072cd51aa85c9ee6334cc99158/raw/bda0f760878d97213196d634be9b53a089e796ea/yarn-site.xml
[exam@vm1-headnode point23]$ sed -i 's|%YARN_RESOURCE_MANAGER_HOSTNAME%|vm1-headnode|' yarn-site.xml
[exam@vm1-headnode point23]$ sed -i 's|%NODE_MANAGER_LOCAL_DIR%|/opt/mount1/nodemanager-local-dir,/opt/mount2/nodemanager-local-dir|' yarn-site.xml
[exam@vm1-headnode point23]$ sed -i 's|%NODE_MANAGER_LOG_DIR%|/opt/mount1/nodemanager-log-dir,/opt/mount2/nodemanager-log-dir|' yarn-site.xml
```
> Проверяем результат.
```bash
[exam@vm1-headnode point23]$ cat yarn-site.xml |grep " <value>" | tail -2
        <value>/opt/mount1/nodemanager-local-dir,/opt/mount2/nodemanager-local-dir</value>
        <value>/opt/mount1/nodemanager-log-dir,/opt/mount2/nodemanager-log-dir</value>
```
> Перекидываем все измененные файлы в директорию.
```bash
[exam@vm1-headnode point23]$ sudo mv /home/exam/point23/* /usr/local/hadoop/current/hadoop-3.1.2/etc/hadoop/
```
## 24. Создание переменной окружения `HADOOP_HOME`.
#### Задать переменную окружения `HADOOP_HOME` через `/etc/profile`
> Редактируем файл `/etc/profile` добавляя строку.
```bash
export HADOOP_HOME=/usr/local/hadoop/current/hadoop-3.1.2
```
> Сохраняем изменения и проверяем результат.
```bash
[exam@vm1-headnode ~]$ source /etc/profile
[exam@vm1-headnode ~]$ env | grep HADOOP_HOME
HADOOP_HOME=/usr/local/hadoop/current/hadoop-3.1.2
```
# Для `VM1` (шаги 25-26):
## 25. Форматирование `HDFS` пользователем `hdfs`.
#### Произвести форматирование `HDFS` (от имени пользователя `hdfs`):
#### •	$HADOOP_HOME/bin/hdfs namenode -format cluster1
> После вводы команды видим ошибку.
```bash
 [exam@vm1-headnode ~]$  sudo su -l hdfs -c "$HADOOP_HOME/bin/hdfs namenode -format cluster1"
WARNING: /usr/local/hadoop/current/hadoop-3.1.2/logs does not exist. Creating.
mkdir: cannot create directory ‘/usr/local/hadoop/current/hadoop-3.1.2/logs’: Permission denied
ERROR: Unable to create /usr/local/hadoop/current/hadoop-3.1.2/logs. Aborting. 
```
> Как я понял необходимо предоставить права. Прописываем права, но так как мы видим, что будет создаваться папка то сразу её создаем (так как если не создать появиться другая ошибка при запуске далее) что бы те права, которые мы пропишем распространялись и на неё тоже.
```bash
[exam@vm1-headnode ~]$ sudo mkdir /opt/hadoop-3.1.2/hadoop-3.1.2/logs
[exam@vm1-headnode ~]$ sudo chown -R :hadoop /opt/hadoop-3.1.2/hadoop-3.1.2/logs
[exam@vm1-headnode ~]$ sudo chmod -R g+wxr /opt/hadoop-3.1.2/hadoop-3.1.2/logs
```
> Теперь пробуем снова.
```bash
[exam@vm1-headnode ~]$  sudo su -l hdfs -c "$HADOOP_HOME/bin/hdfs namenode -format cluster1"
2022-01-17 19:36:14,826 INFO namenode.NameNode: STARTUP_MSG:
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = vm1-headnode/192.168.100.11
STARTUP_MSG:   args = [-format, cluster1]
STARTUP_MSG:   version = 3.1.2 
………etc………
```
## 26. Запуск демонов сервисов `Hadoop`.
#### Запустить демоны сервисов Hadoop:
#### Для запуска `Namenode` (от имени пользователя `hdfs`):
#### •	$HADOOP_HOME/bin/hdfs --daemon start namenode
#### Для запуска `Resource Manager` (от имени пользователя `yarn`):
#### •	$HADOOP_HOME/bin/yarn --daemon start resourcemanager
> Запускаем `Namenode` от пользователя `hdfs`.
```bash
 [exam@vm1-headnode ~]$  sudo su -l hdfs -c "$HADOOP_HOME/bin/hdfs --daemon start namenode"
```
> Запускаем `resourcemanager` от пользователя `yarn`.
```bash
[exam@vm1-headnode ~]$  sudo su -l yarn -c "$HADOOP_HOME/bin/yarn --daemon start resourcemanager
```
# Для `VM2` (шаг 27):
## 27. `Запуск Datanode` и `Node Manager`.
#### Запустить демоны сервисов:
#### Для запуска Datanode (от имени `hdfs`):
#### •	$HADOOP_HOME/bin/hdfs --daemon start datanode
#### Для запуска Node Manager (от имени `yarn`):
#### •	$HADOOP_HOME/bin/yarn --daemon start nodemanager
> Так как при попытке запустить `datanode` выводиться та же ошибка что была на первой виртуальной машине произведём те же действия по предоставлению прав.
```bash
[exam@vm2-worker ~]$ sudo mkdir /opt/hadoop-3.1.2/hadoop-3.1.2/logs
[exam@vm2-worker ~]$ sudo chown -R :hadoop /opt/hadoop-3.1.2/hadoop-3.1.2/logs
[exam@vm2-worker ~]$ sudo chmod -R g+wxr /opt/hadoop-3.1.2/hadoop-3.1.2/logs
```
> Запускаем `datanode` от пользователя `hdfs`.
```bash
[exam@vm2-worker ~]$  sudo su -l hdfs -c "$HADOOP_HOME/bin/hdfs --daemon start datanode"
```
> Запускаем `nodemanager` от пользователя `yarn`.
```bash
[exam@vm2-worker ~]$  sudo su -l yarn -c "$HADOOP_HOME/bin/yarn --daemon start nodemanager"
```
## 28. Проверка доступности Web-интефейсов.
#### Проверить доступность Web-интефейсов `HDFS Namenode` и `YARN Resource Manager` по портам `9870` и `8088` соответственно (`VM1`). << порты должны быть доступны с хостовой системы.
Для того что бы `firewall` не блокировал наши Web-интерфейсы разрешаем необходимые нам порты следующими командами.
```bash
[exam@vm1-headnode ~]$ sudo firewall-cmd --add-port=8088/tcp
success
[exam@vm1-headnode ~]$ sudo firewall-cmd --add-port=9870/tcp
Success
```
> Далее, что бы наш первый компьютер мог связаться (получать данные) от второго пропишем `rich` правило для сети мало ли захотим в наш кластер добавить ещё ПК.
```bash
[exam@vm1-headnode ~]$  sudo firewall-cmd --permanent --zone=public --add-rich-rule='rule family=ipv4 source address=192.168.100.11/24 accept'
```
> Последним этапом все сохраняем, перезагружаем `firewall` и смотрим на результат.
```bash
[exam@vm1-headnode ~]$ sudo firewall-cmd --reload
Success
[exam@vm1-headnode ~]$ sudo firewall-cmd --runtime-to-permanent
success
[exam@vm1-headnode ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3
  sources:
  services: dhcpv6-client ssh
  ports: 8088/tcp 9870/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
        rule family="ipv4" source address="192.168.100.11/24" accept
```
> С `firewall` закончили смотрим на результат. Запускаем браузер и прописываем в строке поиска следующие `URL` адреса в разных вкладках:
> - http://192.168.100.11:8088

![image](https://user-images.githubusercontent.com/95025513/150648341-bc7e2e55-929a-42a0-93f2-7c80d67072b4.png)
> - http://192.168.100.11:9870

![image](https://user-images.githubusercontent.com/95025513/150648272-6a4fe73f-6cf2-4ea0-8fe2-1ac8918f1611.png)
> На первом и втором Web-интерфейсе можно наблюдать наш второй ПК `vm2-worker`.
## 29. настройка управление запуском при помощи `systemd`.
#### Настроить управление запуском каждого компонента `Hadoop` при помощи `systemd` (используя юниты-сервисы).
##### Начнем с «namenode» на `vm1-headnode` создаем файл в `/etc/systemd/system/` и прописываем следующие строчки.
```bash
[Unit]
Description=Hadoop DFS namenode and datanode

[Service]
User=hdfs
Group=hadoop
Type=forking
ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/hadoop-daemon.sh start namenode
ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/hadoop-daemon.sh stop namenode
WorkingDirectory=/usr/local/hadoop/current

[Install]
WantedBy=multi-user.target
```
#### Далее создаем файл для `resourcemanager` в той же директории на `vm1-headnode`.
```bash
[Unit]
Description=Hadoop DFS namenode and datanode

[Service]
User=yarn
Group=hadoop
Type=forking
ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh start resourcemanager
ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh stop resourcemanager
WorkingDirectory=/usr/local/hadoop/current

[Install]
WantedBy=multi-user.target
```
#### Далее перезагружаем `systemctl daemon` и после саму систему.
```bash
[exam@vm1-headnode ~]$ sudo systemctl daemon-reload
[exam@vm1-headnode ~]$ sudo reboot
```
#### Теперь запускаем наши компоненты и проверяем их статусы.
```bash
[exam@vm1-headnode ~]$ sudo systemctl start namenode
[exam@vm1-headnode ~]$ sudo systemctl start resourcemanager
[exam@vm1-headnode ~]$ sudo systemctl status namenode
● namenode.service - Hadoop DFS namenode and datanode
   Loaded: loaded (/etc/systemd/system/namenode.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-01-17 19:51:52 MSK; 7min ago
 Main PID: 1703 (java)
   CGroup: /system.slice/namenode.service
           └─1703 /usr/lib/jvm/jre-1.8.0-openjdk/bin/java -Dproc_namenode -Djava.net.preferIPv4Stack=true -Dhdfs.audit.logger=INFO,NullAppender -Dhadoop.security.logger=INFO,RFAS -Dyarn.log.dir=/usr/local/hadoop/current/hadoop-3.1.2/l...

Jan 17 19:51:50 vm1-headnode systemd[1]: Starting Hadoop DFS namenode and datanode...
Jan 17 19:51:50 vm1-headnode hadoop-daemon.sh[1641]: WARNING: Use of this script to start HDFS daemons is deprecated.
Jan 17 19:51:50 vm1-headnode hadoop-daemon.sh[1641]: WARNING: Attempting to execute replacement "hdfs --daemon start" instead.
Jan 17 19:51:52 vm1-headnode systemd[1]: Started Hadoop DFS namenode and datanode.
[exam@vm1-headnode ~]$ sudo systemctl status resourcemanager
● resourcemanager.service - Hadoop DFS namenode and datanode
   Loaded: loaded (/etc/systemd/system/resourcemanager.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-01-17 19:56:25 MSK; 2min 39s ago
  Process: 2122 ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh stop resourcemanager (code=exited, status=0/SUCCESS)
  Process: 2194 ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh start resourcemanager (code=exited, status=0/SUCCESS)
 Main PID: 2256 (java)
   CGroup: /system.slice/resourcemanager.service
           └─2256 /usr/lib/jvm/jre-1.8.0-openjdk/bin/java -Dproc_resourcemanager -Djava.net.preferIPv4Stack=true -Dservice.libdir=/usr/local/hadoop/current/hadoop-3.1.2/share/hadoop/yarn,/usr/local/hadoop/current/hadoop-3.1.2/share/ha...

Jan 17 19:56:23 vm1-headnode systemd[1]: Starting Hadoop DFS namenode and datanode...
Jan 17 19:56:23 vm1-headnode yarn-daemon.sh[2194]: WARNING: Use of this script to start YARN daemons is deprecated.
Jan 17 19:56:23 vm1-headnode yarn-daemon.sh[2194]: WARNING: Attempting to execute replace
```
#### Приступим к `vm2-worker` и к компоненту `datanode` так же создаем файл в директории `/etc/systemd/system/` и прописываем следующие строчки.
```bash
[Unit]
Description=Hadoop DFS namenode and datanode

[Service]
User=hdfs
Group=hadoop
Type=forking
ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/hadoop-daemon.sh start datanode
ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/hadoop-daemon.sh stop datanode
WorkingDirectory=/usr/local/hadoop/current

[Install]
WantedBy=multi-user.target
```
#### Теперь для `nodemanager` так же создаем файл и прописываем следующее.
```bash
[Unit]
Description=Hadoop DFS namenode and datanode

[Service]
User=yarn
Group=hadoop
Type=forking
ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh start nodemanager
ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh stop nodemanager
WorkingDirectory=/usr/local/hadoop/current

[Install]
WantedBy=multi-user.target
```
#### Теперь перезагружаем `systemctl daemon` запускаем наши компоненты и проверяем их статусы.
```bash
[exam@vm2-worker ~]$ sudo systemctl start datanode
[exam@vm2-worker ~]$ sudo systemctl start nodemanager
[exam@vm2-worker ~]$ sudo systemctl status datanode
● datanode.service - Hadoop DFS namenode and datanode
   Loaded: loaded (/etc/systemd/system/datanode.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-01-17 20:01:46 MSK; 4s ago
  Process: 3048 ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/hadoop-daemon.sh stop datanode (code=exited, status=0/SUCCESS)
  Process: 3143 ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/hadoop-daemon.sh start datanode (code=exited, status=0/SUCCESS)
 Main PID: 3205 (java)
   CGroup: /system.slice/datanode.service
           └─3205 /usr/lib/jvm/jre-1.8.0-openjdk/bin/java -Dproc_datanode -Djava.net.preferIPv4Stack=true -Dhadoop.security.logger=ERROR,RFAS -Dyarn.log.dir=/usr/local/hadoop/current/hadoop-3.1.2/logs -Dyarn.log.file=hadoop-hdfs-datan...

Jan 17 20:01:44 vm2-worker systemd[1]: Starting Hadoop DFS namenode and datanode...
Jan 17 20:01:44 vm2-worker hadoop-daemon.sh[3143]: WARNING: Use of this script to start HDFS daemons is deprecated.
Jan 17 20:01:44 vm2-worker hadoop-daemon.sh[3143]: WARNING: Attempting to execute replacement "hdfs --daemon start" instead.
Jan 17 20:01:46 vm2-worker systemd[1]: Started Hadoop DFS namenode and datanode.
[exam@vm2-worker ~]$ sudo systemctl status nodemanager
● nodemanager.service - Hadoop DFS namenode and datanode
   Loaded: loaded (/etc/systemd/system/nodemanager.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2022-01-17 19:55:42 MSK; 6min ago
  Process: 2616 ExecStop=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh stop nodemanager (code=exited, status=0/SUCCESS)
  Process: 2830 ExecStart=/usr/local/hadoop/current/hadoop-3.1.2/sbin/yarn-daemon.sh start nodemanager (code=exited, status=0/SUCCESS)
 Main PID: 2893 (java)
   CGroup: /system.slice/nodemanager.service
           └─2893 /usr/lib/jvm/jre-1.8.0-openjdk/bin/java -Dproc_nodemanager -Djava.net.preferIPv4Stack=true -Dyarn.log.dir=/usr/local/hadoop/current/hadoop-3.1.2/logs -Dyarn.log.file=hadoop-yarn-nodemanager-vm2-worker.log -Dyarn.home...

Jan 17 19:55:40 vm2-worker systemd[1]: Starting Hadoop DFS namenode and datanode...
Jan 17 19:55:40 vm2-worker yarn-daemon.sh[2830]: WARNING: Use of this script to start YARN daemons is deprecated.
Jan 17 19:55:40 vm2-worker yarn-daemon.sh[2830]: WARNING: Attempting to execute replacement "yarn --daemon start" instead.
Jan 17 19:55:42 vm2-worker systemd[1]: Started Hadoop DFS namenode and datanode.
```
#### Всё работает.
