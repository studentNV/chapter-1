# Lesson 10

## Home work 19-20

### Boot process

#### 1. enable recovery options for grub, update main configuration file and find new item in grub2 config in /boot.
```bash
[sit2@localhost ~]$ sudo vi /etc/default/grub
GRUB_DISABLE_RECOVERY="false"
[sit2@localhost ~]$ sudo grub2-mkconfig
[sit2@localhost ~]$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-1160.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-1160.el7.x86_64.img
Found linux image: /boot/vmlinuz-0-rescue-3e1adfda93b3104e955c89f5f9da333a
Found initrd image: /boot/initramfs-0-rescue-3e1adfda93b3104e955c89f5f9da333a.img
done
``` 
> add this item
```bash
}
menuentry 'CentOS Linux (0-rescue-3e1adfda93b3104e955c89f5f9da333a) 7 (Core) (recovery mode)' --class centos --class gnu-linux --class gnu --class os --unrestricted $menuentry_id_option 'gnulinux-0-rescue-3e1adfda93b3104e955c89f5f9da333a-recovery-dbc5651d-5026-48c1-b5c0-533e72657840' {
        load_video
        insmod gzio
        insmod part_msdos
        insmod xfs
        set root='hd0,msdos1'
        if [ x$feature_platform_search_hint = xy ]; then
          search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 --hint='hd0,msdos1'  cef6b1ab-220d-4e0c-afa6-7e0d4a80f829
        else
          search --no-floppy --fs-uuid --set=root cef6b1ab-220d-4e0c-afa6-7e0d4a80f829
        fi
        linux16 /vmlinuz-0-rescue-3e1adfda93b3104e955c89f5f9da333a root=/dev/mapper/centos-root ro single crashkernel=auto spectre_v2=retpoline rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet
        initrd16 /initramfs-0-rescue-3e1adfda93b3104e955c89f5f9da333a.img
}
```
#### 2. modify option vm.dirty_ratio:
   - using echo utility
   ```bash
   [sit2@localhost ~]$ sudo bash -c "echo "10" > /proc/sys/vm/dirty_ratio"
   [sit2@localhost ~]$  sudo sysctl -a | grep vm.dirty_ratio
   sysctl: reading key "net.ipv6.conf.all.stable_secret"
   sysctl: reading key "net.ipv6.conf.default.stable_secret"
   sysctl: reading key "net.ipv6.conf.enp0s3.stable_secret"
   sysctl: reading key "net.ipv6.conf.enp0s8.stable_secret"
   sysctl: reading key "net.ipv6.conf.lo.stable_secret"
   vm.dirty_ratio = 10

   ```
   - using sysctl utility
   ```bash
   [sit2@localhost ~]$ sudo sysctl -w vm.dirty_ratio=20
   vm.dirty_ratio = 20
   [sit2@localhost ~]$ sudo sysctl -a | grep vm.dirty_ratio
   sysctl: reading key "net.ipv6.conf.all.stable_secret"
   sysctl: reading key "net.ipv6.conf.default.stable_secret"
   sysctl: reading key "net.ipv6.conf.enp0s3.stable_secret"
   sysctl: reading key "net.ipv6.conf.enp0s8.stable_secret"
   sysctl: reading key "net.ipv6.conf.lo.stable_secret"
   vm.dirty_ratio = 20
   ```
   - using sysctl configuration files
   ```bash
   [sit2@localhost ~]$ sudo cat /etc/sysctl.d/99-sysctl.conf
   # sysctl settings are defined through files in
   # /usr/lib/sysctl.d/, /run/sysctl.d/, and /etc/sysctl.d/.
   #
   # Vendors settings live in /usr/lib/sysctl.d/.
   # To override a whole file, create a new file with the same in
   # /etc/sysctl.d/ and put new settings there. To override
   # only specific settings, add a file with a lexically later
   # name in /etc/sysctl.d/ and put new settings there.
   #
   # For more information, see sysctl.conf(5) and sysctl.d(5).
   vm.dirty_ratio = 30
   [sit2@localhost ~]$ sudo sysctl -a | grep vm.dirty_ratio
   sysctl: reading key "net.ipv6.conf.all.stable_secret"
   sysctl: reading key "net.ipv6.conf.default.stable_secret"
   sysctl: reading key "net.ipv6.conf.enp0s3.stable_secret"
   sysctl: reading key "net.ipv6.conf.enp0s8.stable_secret"
   sysctl: reading key "net.ipv6.conf.lo.stable_secret"
   vm.dirty_ratio = 30
   ```

### Selinux

#### Disable selinux using kernel cmdline
```bash
[sit2@localhost ~]$ getenforce
Enforcing
[sit2@localhost ~]$ vi  /etc/default/grub
[sit2@localhost ~]$ sudo vi /etc/default/grub
GRUB_TIMEOUT=5
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto spectre_v2=retpoline rd.lvm.lv=centos/root rd.lvm.lv=centos/swap rhgb quiet selinux=0"
GRUB_DISABLE_RECOVERY="false"
[sit2@localhost ~]$ sudo grub2-mkconfig -o /boot/grub2/grub.cfg
[sit2@localhost ~]$ getenforce
Disabled
```

### Firewalls

#### 1. Add rule using firewall-cmd that will allow SSH access to your server *only* from network 192.168.56.0/24 and interface enp0s8 (if your network and/on interface name differs - change it accordingly).
```bash
[root@localhost ~]# systemctl status firewalld
[root@localhost ~]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s8
  sources:
  services: dhcpv6-client ssh
  ports:
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[root@localhost ~]# firewall-cmd --zone=public --add-source=192.168.56.0/24 --permanent 
[root@localhost ~]# firewall-cmd --zone=public --add-source=192.168.56.0/24
[root@localhost ~]# firewall-cmd --zone=public --remove-port=22/tcp --permanent 
[root@localhost ~]# firewall-cmd --zone=public --remove-port=22/tcp 
[root@localhost ~]# firewall-cmd --reload
[root@localhost ~]# sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s8
  sources: 192.168.56.0/24
  services: dhcpv6-client  ssh
  ports: 22/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

#### 2. Shutdown firewalld and add the same rules via iptables.
```bash
[root@localhost ~]# systemctl stop firewalld
[root@localhost ~]# yum -y install iptables-services
[root@localhost ~]# systemctl start iptables
[root@localhost ~]# systemctl status iptables
● iptables.service - IPv4 firewall with iptables
   Loaded: loaded (/usr/lib/systemd/system/iptables.service; disabled; vendor preset: disabled)
   Active: active (exited) since Thu 2021-12-30 15:11:03 EST; 2s ago
  Process: 2144 ExecStart=/usr/libexec/iptables/iptables.init start (code=exited, status=0/SUCCESS)
 Main PID: 2144 (code=exited, status=0/SUCCESS)

Dec 30 15:11:03 localhost.localdomain systemd[1]: Starting IPv4 firewall with iptables...
Dec 30 15:11:03 localhost.localdomain iptables.init[2144]: iptables: Applying firewall rules: [  OK  ]
Dec 30 15:11:03 localhost.localdomain systemd[1]: Started IPv4 firewall with iptables.
[root@localhost ~]# iptables -vnL
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
  114  8144 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    0     0 ACCEPT     icmp --  *      *       0.0.0.0/0            0.0.0.0/0
    0     0 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0
    0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
  334 97194 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT 70 packets, 8248 bytes)
 pkts bytes target     prot opt in     out     source               destination
[root@localhost ~]# vi /etc/sysconfig/iptables
-A INPUT -i enp0s8 -p tcp -s 192.168.56.0/24 -m tcp --dport 22 -j ACCEPT
[root@localhost ~]# iptables-restore  < /etc/sysconfig/iptables
[root@localhost ~]# iptables -vnL
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    5   360 ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
    0     0 ACCEPT     icmp --  *      *       0.0.0.0/0            0.0.0.0/0
    0     0 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0
    0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            state NEW tcp dpt:22
    0     0 ACCEPT     tcp  --  enp0s8 *       192.168.56.0/24      0.0.0.0/0            tcp dpt:22
   21  6111 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 REJECT     all  --  *      *       0.0.0.0/0            0.0.0.0/0            reject-with icmp-host-prohibited

Chain OUTPUT (policy ACCEPT 4 packets, 384 bytes)
 pkts bytes target     prot opt in     out     source               destination
[root@localhost ~]#
```
