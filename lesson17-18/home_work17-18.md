# Lesson 9

## Home work 17-18

#### 1. add secondary ip address to you second network interface enp0s8. Each point must be presented with commands and showing that new address was applied to the interface. To repeat adding address for points 2 and 3 address must be deleted (please add deleting address to you homework log) Methods:
##### a. using ip utility (stateless)
```bash
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86239sec preferred_lft 86239sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 597sec preferred_lft 597sec
[sit2@localhost ~]$ sudo ip addr add  192.168.56.104/24 dev enp0s8
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86213sec preferred_lft 86213sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 571sec preferred_lft 571sec
    inet 192.168.56.104/24 scope global secondary enp0s8
       valid_lft forever preferred_lft forever
```

> Deleted
```bash
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86344sec preferred_lft 86344sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet 192.168.56.104/24 scope global secondary enp0s8
       valid_lft forever preferred_lft forever
[sit2@localhost ~]$ sudo systemctl restart network
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86395sec preferred_lft 86395sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
```

##### b. using centos network configuration file (statefull)
> file /etc/sysconfig/network-scripts/ifcfg-enp0s8:1
```bash
DEVICE=enp0s8:1
BOOTPROTO=static
IPADDR=192.168.56.104
NETMASK=255.255.255.0
ONBOOT=yes
```

```bash
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86210sec preferred_lft 86210sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 563sec preferred_lft 563sec
[sit2@localhost ~]$ sudo ifup enp0s8:1
Determining if ip address 192.168.56.104 is already in use for device enp0s8...
RTNETLINK answers: File exists
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86184sec preferred_lft 86184sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 537sec preferred_lft 537sec
    inet 192.168.56.104/24 brd 192.168.56.255 scope global secondary enp0s8
       valid_lft forever preferred_lft forever
[sit2@localhost ~]$ systemctl restart network
```

> Deleted
```bash
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86209sec preferred_lft 86209sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 598sec preferred_lft 598sec
    inet 192.168.56.104/24 brd 192.168.56.255 scope global secondary noprefixroute enp0s8:1
       valid_lft forever preferred_lft forever
[sit2@localhost ~]$ sudo ifdown enp0s8
Device 'enp0s8' successfully disconnected.
[sit2@localhost ~]$ sudo rm /etc/sysconfig/network-scripts/ifcfg-enp0s8:1
[sit2@localhost ~]$ sudo ifup enp0s8
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86147sec preferred_lft 86147sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 598sec preferred_lft 598sec
```
##### c. using nmcli utility (statefull)
```bash
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86326sec preferred_lft 86326sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute dynamic enp0s8
       valid_lft 598sec preferred_lft 598sec
[sit2@localhost ~]$ sudo nmcli dev mod enp0s8 ipv4.method manual ipv4.addresses "192.168.56.103/24,192.168.56.104/24"
Connection successfully reapplied to device 'enp0s8'.
[sit2@localhost ~]$ ip -4 a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
       valid_lft 86305sec preferred_lft 86305sec
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    inet 192.168.56.103/24 brd 192.168.56.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet 192.168.56.104/24 brd 192.168.56.255 scope global secondary noprefixroute enp0s8
       valid_lft forever preferred_lft forever
[sit2@localhost ~]$ sudo nmcli con mod enp0s8 ipv4.method manual ipv4.addresses "192.168.56.103/24,192.168.56.104/24"
[sit2@localhost ~]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8
UUID=ba53129b-c5de-4451-b49b-0a4bae264320
DEVICE=enp0s8
ONBOOT=no
IPADDR=192.168.56.103
PREFIX=24
IPADDR1=192.168.56.104
PREFIX1=24
```

> Deleted
```bash
[sit2@localhost ~]$ sudo nmcli dev mod enp0s8 ipv4.method manual ipv4.addresses "192.168.56.103/24"
Connection successfully reapplied to device 'enp0s8'.
[sit2@localhost ~]$ sudo nmcli con mod enp0s8 ipv4.method manual ipv4.addresses "192.168.56.103/24"
[sit2@localhost ~]$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s8
UUID=ba53129b-c5de-4451-b49b-0a4bae264320
DEVICE=enp0s8
ONBOOT=yes
IPADDR=192.168.56.103
PREFIX=24

```
#### 2. You should have a possibility to use ssh client to connect to your node using new address from previous step. Run tcpdump in separate tmux session or separate connection before starting ssh client and capture packets that are related to this ssh connection. Find packets that are related to TCP session establish.
```bash
[sit2@localhost ~]$ sudo tcpdump -vvv -i enp0s8 dst 192.168.56.103
tcpdump: listening on enp0s8, link-type EN10MB (Ethernet), capture size 262144 bytes
09:01:51.902598 IP (tos 0x0, ttl 128, id 365, offset 0, flags [DF], proto TCP (6), length 52)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [S], cksum 0x4101 (correct), seq 3790498554, win 64240, options [mss 1460,nop,wscale 8,nop,nop,sackOK], length 0
09:01:51.902827 IP (tos 0x0, ttl 128, id 366, offset 0, flags [DF], proto TCP (6), length 40)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [.], cksum 0xd79a (correct), seq 3790498555, ack 1797265894, win 8212, length 0
09:01:51.908962 IP (tos 0x0, ttl 128, id 367, offset 0, flags [DF], proto TCP (6), length 68)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [P.], cksum 0xe165 (correct), seq 0:28, ack 1, win 8212, length 28
09:01:51.917908 IP (tos 0x0, ttl 128, id 368, offset 0, flags [DF], proto TCP (6), length 1296)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [P.], cksum 0x8423 (correct), seq 28:1284, ack 22, win 8212, length 1256
09:01:51.920325 IP (tos 0x0, ttl 128, id 369, offset 0, flags [DF], proto TCP (6), length 88)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [P.], cksum 0x3f5e (correct), seq 1284:1332, ack 1302, win 8207, length 48
09:01:51.933171 IP (tos 0x0, ttl 128, id 370, offset 0, flags [DF], proto TCP (6), length 120)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [P.], cksum 0x2191 (correct), seq 1332:1412, ack 1606, win 8212, length 80
09:01:51.973922 IP (tos 0x0, ttl 128, id 371, offset 0, flags [DF], proto TCP (6), length 40)
    192.168.56.1.62565 > localhost.localdomain.ssh: Flags [.], cksum 0xcb91 (correct), seq 1412, ack 1670, win 8212, length 0
```

#### 3. Close session. Find in tcpdump output packets that are related to TCP session closure.
```bash
09:03:32.163325 IP (tos 0x0, ttl 128, id 417, offset 0, flags [DF], proto TCP (6), length 104)
    192.168.56.1.62599 > localhost.localdomain.ssh: Flags [P.], cksum 0x51dd (correct), seq 2388:2452, ack 3142, win 8212, length 64
09:03:32.164690 IP (tos 0x0, ttl 128, id 418, offset 0, flags [DF], proto TCP (6), length 40)
    192.168.56.1.62599 > localhost.localdomain.ssh: Flags [.], cksum 0x9b4a (correct), seq 2452, ack 3382, win 8211, length 0
09:03:32.167863 IP (tos 0x0, ttl 128, id 419, offset 0, flags [DF], proto TCP (6), length 40)
    192.168.56.1.62599 > localhost.localdomain.ssh: Flags [F.], cksum 0x9b49 (correct), seq 2452, ack 3382, win 8211, length 0
09:03:32.168360 IP (tos 0x0, ttl 128, id 420, offset 0, flags [DF], proto TCP (6), length 40)
    192.168.56.1.62599 > localhost.localdomain.ssh: Flags [.], cksum 0x9b48 (correct), seq 2453, ack 3383, win 8211, length 0
```

#### 4. run tcpdump and request any http site in separate session. Find HTTP request and answer packets with ASCII data in it.  Tcpdump command must be as strict as possible to capture only needed packages for this http request.
> Enother screen `curl http://nginx.org/`
```bash
[sit2@localhost ~]$ sudo tcpdump -A -i enp0s3 "src 10.0.2.15 and ip proto \tcp"
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes
10:53:03.573867 IP 10.0.2.15.50424 > 3.125.197.172.80: Flags [S], seq 3099794766, win 29200, options [mss 1460,sackOK,TS val 835598 ecr 0,nop,wscale 7], length 0
E..<.@@.@..C
....}.....P...N......r..f.........
............
10:53:03.648861 IP 3.125.197.172.80 > 10.0.2.15.50424: Flags [S.], seq 1169280001, ack 3099794767, win 65535, options [mss 1460], length 0
E..,....@....}..
....P..E......O`.............
10:53:03.648971 IP 10.0.2.15.50424 > 3.125.197.172.80: Flags [.], ack 1, win 29200, length 0
E..(.A@.@..V
....}.....P...OE...P.r..R..
10:53:03.649449 IP 10.0.2.15.50424 > 3.125.197.172.80: Flags [P.], seq 1:74, ack 1, win 29200, length 73: HTTP: GET / HTTP/1.1
E..q.B@.@...
....}.....P...OE...P.r.....GET / HTTP/1.1
User-Agent: curl/7.29.0
Host: nginx.org
Accept: */*


10:53:03.650143 IP 3.125.197.172.80 > 10.0.2.15.50424: Flags [.], ack 74, win 65535, length 0
E..(....@....}..
....P..E.......P...-D........
10:53:03.724047 IP 3.125.197.172.80 > 10.0.2.15.50424: Flags [P.], seq 1:1413, ack 74, win 65535, length 1412: HTTP: HTTP/1.1 200 OK
E.......@..=.}..
....P..E.......P....G..HTTP/1.1 200 OK
Server: nginx/1.19.10
Date: Sat, 25 Dec 2021 15:53:03 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 12368
Last-Modified: Mon, 13 Dec 2021 14:45:41 GMT
Connection: keep-alive
Keep-Alive: timeout=15
ETag: "61b75c95-3050"
Accept-Ranges: bytes
```
