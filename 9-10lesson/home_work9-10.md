# Lesson 5

## Home work 9-10

### Task 1: Processes

#### 1. Run a sleep command three times at different intervals
```bash
[sitis@localhost ~]$ sleep 10000 &
[1] 1650
[sitis@localhost ~]$ sleep 20000 &
[2] 1651
[sitis@localhost ~]$ sleep 30000 &
[3] 1652
```
#### 2. Send a SIGSTOP signal to all of them in three different ways.
```bash
[sitis@localhost ~]$ ps aux | grep sleep
sitis     1650  0.0  0.0 108056   360 pts/1    S    21:54   0:00 sleep 10000
sitis     1651  0.0  0.0 108056   360 pts/1    S    21:54   0:00 sleep 20000
sitis     1652  0.0  0.0 108056   356 pts/1    S    21:54   0:00 sleep 30000
[sitis@localhost ~]$ kill -19 %1
[sitis@localhost ~]$ kill -19 1651
press buttons ^z
```
#### 3. Check their statuses with a job command
```bash
[sitis@localhost ~]$ jobs
[1]-  Stopped                 sleep 10000
[2]   Stopped                 sleep 20000
[3]+  Stopped                 sleep 30000
```
#### 4. Terminate one of them. (Any)
```bash
[sitis@localhost ~]$ jobs
[1]-  Terminated              sleep 10000
[2]   Stopped                 sleep 20000
[3]+  Stopped                 sleep 30000
```
#### 5. To other send a SIGCONT in two different ways.
```bash
[sitis@localhost ~]$ kill -18 %2
[sitis@localhost ~]$ kill -18 1652
[sitis@localhost ~]$ jobs
[2]-  Running                 sleep 20000 &
[3]+  Running                 sleep 30000 &
```
#### 6. Kill one by PID and the second one by job ID
```bash
[sitis@localhost ~]$ kill -15 %2
[2]-  Terminated              sleep 20000
[sitis@localhost ~]$ kill -15 1652
[sitis@localhost ~]$ jobs -l
```

### Task 2: systemd

#### 1. Write two daemons: one should be a simple daemon and do sleep 10 after a start and then do echo 1 > /tmp/homework, the second one should be oneshot and do echo 2 > /tmp/homework without any sleep
> First script
```bash
#!/bin/bash
sleep 10
echo 1 > /tmp/homework
```
> Second script
```bash
#!/bin/bash
echo 2 > /tmp/homework
```
> File `/etc/systemd/system/simpleDaemon.service`
```bash
[Unit]
Description=sleep 10 after a start and then do echo 1 > /tmp/homework

[Service]
Type=simple
ExecStart=/home/sitis/lesson5/simpleDaemon

[Install]
WantedBy=multi-user.target
```
> File `/etc/systemd/system/oneshotDaemon.service`
```bash
[Unit]
Description=Do echo 2 > /tmp/homework

[Service]
Type=simple
ExecStart=/home/sitis/lesson5/oneshotDaemon

[Install]
WantedBy=multi-user.target
```
#### 2. Make the second depended on the first one (should start only after the first)
> File `/etc/systemd/system/oneshotDaemon.service`
```bash
[Unit]
Description=Do echo 2 > /tmp/homework
Wants=simpleDaemon.service

[Service]
Type=oneshot
ExecStart=/home/sitis/lesson5/oneshotDaemon

[Install]
WantedBy=multi-user.target
```
#### 3. Write a timer for the second one and configure it to run on 01.01.2019 at 00:00
> File `/etc/systemd/system/oneshotDaemon.timer`
```bash
[Unitt]
Description=Do echo 2 > /tmp/homework

[Timer]
OnCalendar=2022-01-01 00:00:00
```
#### 4. Start all daemons and timer, check their statuses, timer list and /tmp/homework
> The date has been changed since `2019` has already passed and the date was not displayed
```bash
[sitis@localhost lesson5]$ sudo systemctl enable oneshotDaemon.timer
[sitis@localhost lesson5]$ sudo systemctl start oneshotDaemon.timer
[sitis@localhost lesson5]$ systemctl list-timers
NEXT                         LEFT                LAST PASSED UNIT                         ACTIVATES
Sat 2022-01-01 00:00:00 MSK  2 weeks 6 days left n/a  n/a    oneshotDaemon.timer          oneshotDaemon.service
```
```bash
[sitis@localhost lesson5]$ sudo systemctl start oneshotDaemon
[sitis@localhost lesson5]$ cat /tmp/homework
2
[sitis@localhost lesson5]$ cat /tmp/homework
1
[sitis@localhost lesson5]$ sudo systemctl start simpleDaemon
[sitis@localhost lesson5]$ cat /tmp/homework
1
```
#### 5. Stop all daemons and timer
```bash
[sitis@localhost lesson5]$ sudo systemctl stop oneshotDaemon.timer
[sitis@localhost lesson5]$ sudo systemctl disable oneshotDaemon.timer
[sitis@localhost lesson5]$ sudo systemctl stop oneshotDaemon
[sitis@localhost lesson5]$ sudo systemctl stop simpleDaemon
[sitis@localhost lesson5]$ systemctl list-timers
NEXT LEFT LAST PASSED UNIT                         ACTIVATES
n/a  n/a  n/a  n/a    systemd-tmpfiles-clean.timer
```

### Task 3: cron/anacron

#### 1. Create an anacron job which executes a script with echo Hello > /opt/hello and runs every 2 days
> Add this line to file `/etc/anacrontab` 
```bash
2       10      cron.everyTwoDays       echo Hello >/opt/hello
```
#### 2. Create a cron job which executes the same command (will be better to create a script for this) and runs it in 1 minute after system boot.
```bash
[root@localhost sitis]# crontab -e
```
> Add this line
```bash
@reboot sleep 60 && /home/sitis/script
```
#### 3. Restart your virtual machine and check previous job proper execution
```bash
[sitis@localhost ~]$ ll /opt/
total 4
-rw-r--r--. 1 root root 6 Dec 11 02:06 hello
```
-----
### Task 4: lsof

#### 1. Run a sleep command, redirect stdout and stderr into two different files (both of them will be empty).
```bash
[sitis@localhost lesson5]$ sleep 900000 >stdout 2>stderr &
[1] 1728
```
#### 2. Find with the lsof command which files this process uses, also find from which file it gain stdin.
> Files uses process
```bash
[sitis@localhost lesson5]$ lsof -p 1728
COMMAND  PID  USER   FD   TYPE DEVICE  SIZE/OFF     NODE NAME
sleep   1728 sitis  cwd    DIR  253,0        75  8409158 /home/sitis/lesson5
sleep   1728 sitis  rtd    DIR  253,0       224       64 /
sleep   1728 sitis  txt    REG  253,0     33128 12686507 /usr/bin/sleep
sleep   1728 sitis  mem    REG  253,0 106176928 12584456 /usr/lib/locale/locale-archive
sleep   1728 sitis  mem    REG  253,0   2156592     8820 /usr/lib64/libc-2.17.so
sleep   1728 sitis  mem    REG  253,0    163312     8812 /usr/lib64/ld-2.17.so
sleep   1728 sitis    0u   CHR  136,0       0t0        3 /dev/pts/0
sleep   1728 sitis    1w   REG  253,0         0  8409159 /home/sitis/lesson5/stdout
sleep   1728 sitis    2w   REG  253,0         0  8409190 /home/sitis/lesson5/stderr
```
> As I understand it, the line is responsible for reading `0u`
```bash
sleep   1728 sitis    0u   CHR  136,0       0t0        3 /dev/pts/0
```
#### 3. List all ESTABLISHED TCP connections ONLY with lsof
```bash
[sitis@localhost lesson5]$ sudo lsof -iTCP -sTCP:ESTABLISHED
COMMAND  PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
sshd    1501  root    3u  IPv4  18860      0t0  TCP localhost.localdomain:ssh->gateway:54868 (ESTABLISHED)
sshd    1505 sitis    3u  IPv4  18860      0t0  TCP localhost.localdomain:ssh->gateway:54868 (ESTABLISHED)
```
