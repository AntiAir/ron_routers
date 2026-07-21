---
title: ron routers
tags: [7. CCNA]

---

[![hackmd-github-sync-badge](https://hackmd.io/R0593hhvQMyE-UlkgVl5NA/badge)](https://hackmd.io/R0593hhvQMyE-UlkgVl5NA)

ron routers

![image](https://hackmd.io/_uploads/HkmDqaL4fe.png)
```c
buntu@ubuntu-1:~$ systemctl stop NetworkManager.service
ubuntu@ubuntu-1:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:d7:9b:94 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 172.25.250.131/24 brd 172.25.250.255 scope global dynamic noprefixroute ens160
       valid_lft 948sec preferred_lft 948sec
    inet6 fe80::4a74:54b9:f3c6:9651/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:d7:9b:9e brd ff:ff:ff:ff:ff:ff
    altname enp11s0
    inet6 fe80::8ffc:8c36:8b69:998a/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
ubuntu@ubuntu-1:~$ ip address add 192.168.172.1/24 dev ens192
ubuntu@ubuntu-1:~$ sudo apt update

ubuntu@ubuntu-1:~$ sudo apt install openssh-server -y

ubuntu@ubuntu-1:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Created directory '/home/ubuntu/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:8Dq//oFy1dzkym27eNQ1pHGdAYvDmenab4suOJCli4I ubuntu@ubuntu-1
The key's randomart image is:
+---[RSA 3072]----+
|             ...+|
|          . =..+.|
|      .    B .*  |
|       +  .o.= ..|
|      + S ..o o.o|
|     + . oo. o. .|
|.   . * +...o.o  |
|E. . . B . .o+.. |
|  .    .=o+oo++. |
+----[SHA256]-----+

# slave 也同樣要做一次 ssh-keygen -t rsa
    
ubuntu@ubuntu-1:~$ ssh-copy-id ubuntu@192.168.172.145
The authenticity of host '192.168.172.145 (192.168.172.145)' can't be established.
ED25519 key fingerprint is SHA256:wdvm/ZuuHToTKUcUS9zPzf50/15JyhxwmQVNn/wSJpE.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ubuntu@192.168.172.145's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ubuntu@192.168.172.145'"
and check to make sure that only the key(s) you wanted were added.

ubuntu@ubuntu-1:~$ cd .ssh
ubuntu@ubuntu-1:~/.ssh$ ll
total 24
drwx------  2 ubuntu ubuntu 4096  七  17 05:48 ./
drwxr-x--- 15 ubuntu ubuntu 4096  七  17 05:46 ../
-rw-------  1 ubuntu ubuntu 2602  七  17 05:46 id_rsa
-rw-r--r--  1 ubuntu ubuntu  569  七  17 05:46 id_rsa.pub
-rw-------  1 ubuntu ubuntu  978  七  17 05:48 known_hosts
-rw-r--r--  1 ubuntu ubuntu  142  七  17 05:48 known_hosts.old
ubuntu@ubuntu-1:~/.ssh$ 

```
``` controller
ubuntu@ubuntu-1:~$ sudo dhclient ens160
ubuntu@ubuntu-1:~$ sudo apt install ansible -y
ubuntu@ubuntu-1:~$ sudo hostnamectl set-hostname Controller

ubuntu@Controller:~/router$ ansible slave1 -i inventory -m command -a "df -Th"
slave1 | CHANGED | rc=0 >>
Filesystem     Type   Size  Used Avail Use% Mounted on
tmpfs          tmpfs  193M  2.0M  191M   2% /run
/dev/sda3      ext4    39G   16G   21G  44% /
tmpfs          tmpfs  962M     0  962M   0% /dev/shm
tmpfs          tmpfs  5.0M  4.0K  5.0M   1% /run/lock
/dev/sda2      vfat   512M  6.1M  506M   2% /boot/efi
tmpfs          tmpfs  193M  100K  193M   1% /run/user/1000
```
==增加Ansible等待時間==

==ubuntu@Controller:~/router$ export ANSIBLE_TIMEOUT=30==
```
ubuntu@Controller:~/router$ ansible slave1 -i inventory -m shell -a "apt install curl -y" --become --ask-become-pass
BECOME password: 

ubuntu@Controller:~$ touch fromController.txt
ubuntu@Controller:~$ cd router/
ubuntu@Controller:~/router$ ansible slave1 -i inventory -m copy -a "src=/home/ubuntu/fromController.txt dest=/home/ubuntu/Documents/"
slave1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": true,
    "checksum": "da39a3ee5e6b4b0d3255bfef95601890afd80709",
    "dest": "/home/ubuntu/Documents/fromController.txt",
    "gid": 1000,
    "group": "ubuntu",
    "md5sum": "d41d8cd98f00b204e9800998ecf8427e",
    "mode": "0664",
    "owner": "ubuntu",
    "size": 0,
    "src": "/home/ubuntu/.ansible/tmp/ansible-tmp-1784630166.2838233-2701-199741135264746/source",
    "state": "file",
    "uid": 1000
}

```[![hackmd-github-sync-badge](https://hackmd.io/R0593hhvQMyE-UlkgVl5NA/badge)](https://hackmd.io/R0593hhvQMyE-UlkgVl5NA)

ron routers

![image](https://hackmd.io/_uploads/HkmDqaL4fe.png)
```c
buntu@ubuntu-1:~$ systemctl stop NetworkManager.service
ubuntu@ubuntu-1:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens160: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:d7:9b:94 brd ff:ff:ff:ff:ff:ff
    altname enp3s0
    inet 172.25.250.131/24 brd 172.25.250.255 scope global dynamic noprefixroute ens160
       valid_lft 948sec preferred_lft 948sec
    inet6 fe80::4a74:54b9:f3c6:9651/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: ens192: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:0c:29:d7:9b:9e brd ff:ff:ff:ff:ff:ff
    altname enp11s0
    inet6 fe80::8ffc:8c36:8b69:998a/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
ubuntu@ubuntu-1:~$ ip address add 192.168.172.1/24 dev ens192
ubuntu@ubuntu-1:~$ sudo apt update

ubuntu@ubuntu-1:~$ sudo apt install openssh-server -y

ubuntu@ubuntu-1:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ubuntu/.ssh/id_rsa): 
Created directory '/home/ubuntu/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/ubuntu/.ssh/id_rsa
Your public key has been saved in /home/ubuntu/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:8Dq//oFy1dzkym27eNQ1pHGdAYvDmenab4suOJCli4I ubuntu@ubuntu-1
The key's randomart image is:
+---[RSA 3072]----+
|             ...+|
|          . =..+.|
|      .    B .*  |
|       +  .o.= ..|
|      + S ..o o.o|
|     + . oo. o. .|
|.   . * +...o.o  |
|E. . . B . .o+.. |
|  .    .=o+oo++. |
+----[SHA256]-----+

# slave 也同樣要做一次 ssh-keygen -t rsa
    
ubuntu@ubuntu-1:~$ ssh-copy-id ubuntu@192.168.172.145
The authenticity of host '192.168.172.145 (192.168.172.145)' can't be established.
ED25519 key fingerprint is SHA256:wdvm/ZuuHToTKUcUS9zPzf50/15JyhxwmQVNn/wSJpE.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ubuntu@192.168.172.145's password: 

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'ubuntu@192.168.172.145'"
and check to make sure that only the key(s) you wanted were added.

ubuntu@ubuntu-1:~$ cd .ssh
ubuntu@ubuntu-1:~/.ssh$ ll
total 24
drwx------  2 ubuntu ubuntu 4096  七  17 05:48 ./
drwxr-x--- 15 ubuntu ubuntu 4096  七  17 05:46 ../
-rw-------  1 ubuntu ubuntu 2602  七  17 05:46 id_rsa
-rw-r--r--  1 ubuntu ubuntu  569  七  17 05:46 id_rsa.pub
-rw-------  1 ubuntu ubuntu  978  七  17 05:48 known_hosts
-rw-r--r--  1 ubuntu ubuntu  142  七  17 05:48 known_hosts.old
ubuntu@ubuntu-1:~/.ssh$ 

```
``` controller
ubuntu@ubuntu-1:~$ sudo dhclient ens160
ubuntu@ubuntu-1:~$ sudo apt install ansible -y
ubuntu@ubuntu-1:~$ sudo hostnamectl set-hostname Controller

ubuntu@Controller:~/router$ ansible slave1 -i inventory -m command -a "df -Th"
slave1 | CHANGED | rc=0 >>
Filesystem     Type   Size  Used Avail Use% Mounted on
tmpfs          tmpfs  193M  2.0M  191M   2% /run
/dev/sda3      ext4    39G   16G   21G  44% /
tmpfs          tmpfs  962M     0  962M   0% /dev/shm
tmpfs          tmpfs  5.0M  4.0K  5.0M   1% /run/lock
/dev/sda2      vfat   512M  6.1M  506M   2% /boot/efi
tmpfs          tmpfs  193M  100K  193M   1% /run/user/1000
```
==增加Ansible等待時間==

==ubuntu@Controller:~/router$ export ANSIBLE_TIMEOUT=30==
```
ubuntu@Controller:~/router$ ansible slave1 -i inventory -m shell -a "apt install curl -y" --become --ask-become-pass
BECOME password: 

ubuntu@Controller:~$ touch fromController.txt
ubuntu@Controller:~$ cd router/
ubuntu@Controller:~/router$ ansible slave1 -i inventory -m copy -a "src=/home/ubuntu/fromController.txt dest=/home/ubuntu/Documents/"
slave1 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": true,
    "checksum": "da39a3ee5e6b4b0d3255bfef95601890afd80709",
    "dest": "/home/ubuntu/Documents/fromController.txt",
    "gid": 1000,
    "group": "ubuntu",
    "md5sum": "d41d8cd98f00b204e9800998ecf8427e",
    "mode": "0664",
    "owner": "ubuntu",
    "size": 0,
    "src": "/home/ubuntu/.ansible/tmp/ansible-tmp-1784630166.2838233-2701-199741135264746/source",
    "state": "file",
    "uid": 1000
}

```