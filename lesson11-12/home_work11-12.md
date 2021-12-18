# Lesson 5

## Home work 11-12

### Task 1:

#### As a result of each point, you should provide a corresponding command. localhost - your CentOS VM running in VirtualBox remotehost - 18.221.144.175 (public IP) webserver - 172.31.45.237 (private IP)

#### 1.1. SSH to remotehost using username and password provided to you in Slack. Log out from remotehost.
```bash
[sitis@localhost ~]$ ssh Vadim_Novik@18.221.144.175
The authenticity of host '18.221.144.175 (18.221.144.175)' can't be established.
ECDSA key fingerprint is SHA256:Lqk214fPCrlvcsnoj1NGVS+Puxr7lGuEncIdALeLt78.
ECDSA key fingerprint is MD5:01:a1:c8:32:41:5c:9b:4b:0a:cc:f0:4b:7e:66:2b:38.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '18.221.144.175' (ECDSA) to the list of known hosts.
Vadim_Novik@18.221.144.175's password:

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
No packages needed for security; 2 packages available
Run "sudo yum update" to apply all updates.
[Vadim_Novik@ip-172-31-33-155 ~]$ who
Vadim_Novik pts/0        2021-12-18 11:08 (95-52-42-202.dynamic.murmansk.dslavangard.ru)
[Vadim_Novik@ip-172-31-33-155 ~]$ exit
logout
Connection to 18.221.144.175 closed.
[sitis@localhost ~]$
```

#### 1.2. Generate new SSH key-pair on your localhost with name "hw-5" (keys should be created in ~/.ssh folder).
```bash
[sitis@localhost ~]$ ssh-keygen -f ~/.ssh/hw-5
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/sitis/.ssh/hw-5.
Your public key has been saved in /home/sitis/.ssh/hw-5.pub.
The key fingerprint is:
SHA256:TnFsK9kbZ31+Bj1gQAKi2AclJ/Lhf45CJiuqvncQ+9w sitis@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
| . =.+ ....o     |
|  * B .  .. .    |
| . = .  . +  o   |
|   .o    * ..... |
| . oo. .S + o o.o|
|  =o  +o . =   +.|
|.. .+.... .     +|
|o  ..+ E       ..|
|=oo .            |
+----[SHA256]-----+
[sitis@localhost ~]$ ll ~/.ssh/
total 12
-rw-------. 1 sitis sitis 1675 Dec 18 14:09 hw-5
-rw-r--r--. 1 sitis sitis  409 Dec 18 14:09 hw-5.pub
-rw-r--r--. 1 sitis sitis  349 Dec 18 14:07 known_hosts
```

#### 1.3. Set up key-based authentication, so that you can SSH to remotehost without password.
```bash
[sitis@localhost ~]$ ssh-copy-id -i /home/sitis/.ssh/hw-5 Vadim_Novik@18.221.144.175
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/sitis/.ssh/hw-5.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
Vadim_Novik@18.221.144.175's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'Vadim_Novik@18.221.144.175'"
and check to make sure that only the key(s) you wanted were added.

[sitis@localhost ~]$
```

#### 1.4. SSH to remotehost without password. Log out from remotehost.
```bash
[sitis@localhost ~]$ ssh -i /home/sitis/.ssh/hw-5 Vadim_Novik@18.221.144.175
Last login: Sat Dec 18 11:16:40 2021 from 95-52-42-202.dynamic.murmansk.dslavangard.ru

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
No packages needed for security; 2 packages available
Run "sudo yum update" to apply all updates.
[Vadim_Novik@ip-172-31-33-155 ~]$ exit
logout
Connection to 18.221.144.175 closed.
[sitis@localhost ~]$
```

#### 1.5. Create SSH config file, so that you can SSH to remotehost simply running `ssh remotehost` command. As a result, provide output of command `cat ~/.ssh/config`.
```bash
sitis@localhost .ssh]$ cat ~/.ssh/config
Host hw6
        hostname 18.221.144.175
        User Vadim_Novik
        IdentityFile /home/sitis/.ssh/hw-5
```

#### 1.6. Using command line utility (curl or telnet) verify that there are some webserver running on port 80 of webserver.  Notice that webserver has a private network IP, so you can access it only from the same network (when you are on remotehost that runs in the same private network). Log out from remotehost.
```bash
[Vadim_Novik@ip-172-31-33-155 ~]$ curl http://172.31.45.237:80
```

#### 1.7. Using SSH setup port forwarding, so that you can reach webserver from your localhost (choose any free local port you like).
```bash
[sitis@localhost ~]$ ssh -L  8080:172.31.45.237:80 hw6
```
#### 1.8 Like in 1.6, but on localhost using command line utility verify that localhost and port you have specified act like webserver, returning same result as in 1.6.
```bash
[sitis@localhost ~]$ curl http://localhost:8080
```

#### 1.9 (*) Open webserver webpage in browser of your Host machine of VirtualBox (Windows, or Mac, or whatever else you use). You may need to setup port forwarding in settings of VirtualBox.
> I didn't really understand from which host I needed to open it, but I made 2 options.
![image](https://user-images.githubusercontent.com/95025513/146650028-0ed4b3e0-5706-4dec-b1d0-133422b4c346.png)

![image](https://user-images.githubusercontent.com/95025513/146650009-e5794628-b2af-4381-a0d3-2b4dd82ee796.png)


### Task 2:
#### Following tasks should be executed on your localhost as you will need root privileges
#### 2.1. Imagine your localhost has been relocated to Havana. Change the time zone on the localhost to Havana and verify the time zone has been changed properly (may be multiple commands).
```bash
[sitis@localhost ~]$ sudo timedatectl set-timezone America/Havana
[sudo] password for sitis:
[sitis@localhost ~]$ timedatectl | grep -i zone
       Time zone: America/Havana (CST, -0500)
[sitis@localhost ~]$
```

#### 2.2. Find all systemd journal messages on localhost, that were recorded in the last 50 minutes and originate from a system service started with user id 81 (single command).
```bash
[sitis@localhost ~]$ journalctl --since "50 minutes ago" _UID=81
```

#### 2.3. Configure rsyslogd by adding a rule to the newly created configuration file /etc/rsyslog.d/auth-errors.conf to log all security and authentication messages with the priority alert and higher to the /var/log/auth-errors file. Test the newly added log directive with the logger command (multiple commands)
> Add this line to file `/etc/rsyslog.d/auth-errors.conf`
```bash
auth.=alert;auth.=emerg;		/var/log/auth-errors
```
> And then restart `rsyslog`
```bash
[sitis@localhost ~]$ systemctl restart rsyslog
```
> Check the result
```bash
[sitis@localhost ~]$ systemctl restart rsyslog
[sitis@localhost ~]$ logger -p auth.alert "ALART"
[sitis@localhost ~]$ logger -p auth.emerg "EMERG"

Broadcast message from systemd-journald@localhost.localdomain (Sat 2021-12-18 18:58:14 MSK):

sitis[1672]: EMERG

[sitis@localhost ~]$
Message from syslogd@localhost at Dec 18 18:58:14 ...
 sitis:EMERG
^C
[sitis@localhost ~]$ sudo cat /var/log/auth-errors
Dec 18 18:58:14 localhost sitis: ALART
Dec 18 18:58:14 localhost sitis: EMERG
```
