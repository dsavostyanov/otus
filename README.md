# Работа с NFS
```
NFS server:

root@otus2:~# apt install nfs-kernel-server
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  keyutils libnfsidmap1 nfs-common rpcbind
Suggested packages:
  watchdog
The following NEW packages will be installed:
  keyutils libnfsidmap1 nfs-common nfs-kernel-server rpcbind
0 upgraded, 5 newly installed, 0 to remove and 49 not upgraded.
Need to get 569 kB of archives.
After this operation, 2,022 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 libnfsidmap1 amd64 1:2.6.4-3ubuntu5.1 [48.3 kB]
Get:2 http://cy.archive.ubuntu.com/ubuntu noble/main amd64 rpcbind amd64 1.2.6-7ubuntu2 [46.5 kB]
Get:3 http://cy.archive.ubuntu.com/ubuntu noble/main amd64 keyutils amd64 1.6.3-3build1 [56.8 kB]
Get:4 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 nfs-common amd64 1:2.6.4-3ubuntu5.1 [248 kB]
Get:5 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 nfs-kernel-server amd64 1:2.6.4-3ubuntu5.1 [169 kB]
Fetched 569 kB in 1s (508 kB/s)
Selecting previously unselected package libnfsidmap1:amd64.
(Reading database ... 86650 files and directories currently installed.)
Preparing to unpack .../libnfsidmap1_1%3a2.6.4-3ubuntu5.1_amd64.deb ...
Unpacking libnfsidmap1:amd64 (1:2.6.4-3ubuntu5.1) ...
Selecting previously unselected package rpcbind.
Preparing to unpack .../rpcbind_1.2.6-7ubuntu2_amd64.deb ...
Unpacking rpcbind (1.2.6-7ubuntu2) ...
Selecting previously unselected package keyutils.
Preparing to unpack .../keyutils_1.6.3-3build1_amd64.deb ...
Unpacking keyutils (1.6.3-3build1) ...
Selecting previously unselected package nfs-common.
Preparing to unpack .../nfs-common_1%3a2.6.4-3ubuntu5.1_amd64.deb ...
Unpacking nfs-common (1:2.6.4-3ubuntu5.1) ...
Selecting previously unselected package nfs-kernel-server.
Preparing to unpack .../nfs-kernel-server_1%3a2.6.4-3ubuntu5.1_amd64.deb ...
Unpacking nfs-kernel-server (1:2.6.4-3ubuntu5.1) ...
Setting up libnfsidmap1:amd64 (1:2.6.4-3ubuntu5.1) ...
Setting up rpcbind (1.2.6-7ubuntu2) ...
Created symlink /etc/systemd/system/multi-user.target.wants/rpcbind.service → /usr/lib/systemd/system/rpcbind.service.
Created symlink /etc/systemd/system/sockets.target.wants/rpcbind.socket → /usr/lib/systemd/system/rpcbind.socket.
Setting up keyutils (1.6.3-3build1) ...
Setting up nfs-common (1:2.6.4-3ubuntu5.1) ...

Creating config file /etc/idmapd.conf with new version

Creating config file /etc/nfs.conf with new version
info: Selecting UID from range 100 to 999 ...

info: Adding system user `statd' (UID 111) ...
info: Adding new user `statd' (UID 111) with group `nogroup' ...
info: Not creating home directory `/var/lib/nfs'.
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-client.target → /usr/lib/systemd/system/nfs-client.target.
Created symlink /etc/systemd/system/remote-fs.target.wants/nfs-client.target → /usr/lib/systemd/system/nfs-client.target.
auth-rpcgss-module.service is a disabled or a static unit, not starting it.
nfs-idmapd.service is a disabled or a static unit, not starting it.
nfs-utils.service is a disabled or a static unit, not starting it.
proc-fs-nfsd.mount is a disabled or a static unit, not starting it.
rpc-gssd.service is a disabled or a static unit, not starting it.
rpc-statd-notify.service is a disabled or a static unit, not starting it.
rpc-statd.service is a disabled or a static unit, not starting it.
rpc-svcgssd.service is a disabled or a static unit, not starting it.
Setting up nfs-kernel-server (1:2.6.4-3ubuntu5.1) ...
Created symlink /etc/systemd/system/nfs-mountd.service.requires/fsidd.service → /usr/lib/systemd/system/fsidd.service.
Created symlink /etc/systemd/system/nfs-server.service.requires/fsidd.service → /usr/lib/systemd/system/fsidd.service.
Created symlink /etc/systemd/system/nfs-client.target.wants/nfs-blkmap.service → /usr/lib/systemd/system/nfs-blkmap.service.
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-server.service → /usr/lib/systemd/system/nfs-server.service.
nfs-mountd.service is a disabled or a static unit, not starting it.
nfsdcld.service is a disabled or a static unit, not starting it.

Creating config file /etc/exports with new version

Creating config file /etc/default/nfs-kernel-server with new version
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for libc-bin (2.39-0ubuntu8.4) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

root@otus2:~# ss -tnplu
Netid       State        Recv-Q       Send-Q                      Local Address:Port                Peer Address:Port       Process
udp         UNCONN       0            0                                 0.0.0.0:51103                    0.0.0.0:*           users:(("rpc.mountd",pid=2180,fd=12))
udp         UNCONN       0            0                              127.0.0.54:53                       0.0.0.0:*           users:(("systemd-resolve",pid=587,fd=16))
udp         UNCONN       0            0                           127.0.0.53%lo:53                       0.0.0.0:*           users:(("systemd-resolve",pid=587,fd=14))
udp         UNCONN       0            0                   192.168.63.187%enp0s3:68                       0.0.0.0:*           users:(("systemd-network",pid=847,fd=22))
udp         UNCONN       0            0                                 0.0.0.0:111                      0.0.0.0:*           users:(("rpcbind",pid=1665,fd=5),("systemd",pid=1,fd=140))
udp         UNCONN       0            0                               127.0.0.1:649                      0.0.0.0:*           users:(("rpc.statd",pid=2169,fd=5))
udp         UNCONN       0            0                                 0.0.0.0:60272                    0.0.0.0:*           users:(("rpc.mountd",pid=2180,fd=8))
udp         UNCONN       0            0                                 0.0.0.0:56689                    0.0.0.0:*           users:(("rpc.mountd",pid=2180,fd=4))
udp         UNCONN       0            0                                 0.0.0.0:34463                    0.0.0.0:*           users:(("rpc.statd",pid=2169,fd=8))
udp         UNCONN       0            0                                 0.0.0.0:38618                    0.0.0.0:*
udp         UNCONN       0            0                                    [::]:59314                       [::]:*           users:(("rpc.mountd",pid=2180,fd=6))
udp         UNCONN       0            0                                    [::]:111                         [::]:*           users:(("rpcbind",pid=1665,fd=7),("systemd",pid=1,fd=143))
udp         UNCONN       0            0                                    [::]:33205                       [::]:*           users:(("rpc.statd",pid=2169,fd=10))
udp         UNCONN       0            0                                    [::]:58062                       [::]:*           users:(("rpc.mountd",pid=2180,fd=14))
udp         UNCONN       0            0                                    [::]:42137                       [::]:*
udp         UNCONN       0            0                                    [::]:44475                       [::]:*           users:(("rpc.mountd",pid=2180,fd=10))
tcp         LISTEN       0            4096                              0.0.0.0:36235                    0.0.0.0:*           users:(("rpc.mountd",pid=2180,fd=9))
tcp         LISTEN       0            4096                           127.0.0.54:53                       0.0.0.0:*           users:(("systemd-resolve",pid=587,fd=17))
tcp         LISTEN       0            4096                              0.0.0.0:111                      0.0.0.0:*           users:(("rpcbind",pid=1665,fd=4),("systemd",pid=1,fd=138))
tcp         LISTEN       0            64                                0.0.0.0:2049                     0.0.0.0:*
tcp         LISTEN       0            4096                        127.0.0.53%lo:53                       0.0.0.0:*           users:(("systemd-resolve",pid=587,fd=15))
tcp         LISTEN       0            4096                              0.0.0.0:43255                    0.0.0.0:*           users:(("rpc.mountd",pid=2180,fd=5))
tcp         LISTEN       0            4096                              0.0.0.0:59987                    0.0.0.0:*           users:(("rpc.statd",pid=2169,fd=9))
tcp         LISTEN       0            64                                0.0.0.0:35381                    0.0.0.0:*
tcp         LISTEN       0            4096                              0.0.0.0:35655                    0.0.0.0:*           users:(("rpc.mountd",pid=2180,fd=13))
tcp         LISTEN       0            4096                                 [::]:44187                       [::]:*           users:(("rpc.mountd",pid=2180,fd=7))
tcp         LISTEN       0            64                                   [::]:38373                       [::]:*
tcp         LISTEN       0            4096                                 [::]:48727                       [::]:*           users:(("rpc.mountd",pid=2180,fd=11))
tcp         LISTEN       0            4096                                 [::]:51023                       [::]:*           users:(("rpc.statd",pid=2169,fd=11))
tcp         LISTEN       0            4096                                 [::]:111                         [::]:*           users:(("rpcbind",pid=1665,fd=6),("systemd",pid=1,fd=141))
tcp         LISTEN       0            64                                   [::]:2049                        [::]:*
tcp         LISTEN       0            4096                                    *:22                             *:*           users:(("sshd",pid=1313,fd=3),("systemd",pid=1,fd=186))
tcp         LISTEN       0            4096                                 [::]:51519                       [::]:*           users:(("rpc.mountd",pid=2180,fd=15))

root@otus2:~# mkdir -p /srv/share/upload

root@otus2:~# chown -R nobody:nogroup /srv/share

root@otus2:~# chmod 0777 /srv/share/upload

root@otus2:~# cat << EOF > /etc/exports
/srv/share 192.168.63.44/32(rw,sync,root_squash)
EOF

root@otus2:~# exportfs -r
exportfs: /etc/exports [1]: Neither 'subtree_check' or 'no_subtree_check' specified for export "192.168.63.44/32:/srv/share".
  Assuming default behaviour ('no_subtree_check').
  NOTE: this default has changed since nfs-utils version 1.0.x

root@otus2:~# exportfs -s
/srv/share  192.168.63.44/32(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)

NFS client:

root@otus:~# sudo apt install nfs-common
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  keyutils libnfsidmap1 rpcbind
Suggested packages:
  watchdog
The following NEW packages will be installed:
  keyutils libnfsidmap1 nfs-common rpcbind
0 upgraded, 4 newly installed, 0 to remove and 49 not upgraded.
Need to get 400 kB of archives.
After this operation, 1,416 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 libnfsidmap1 amd64 1:2.6.4-3ubuntu5.1 [48.3 kB]
Get:2 http://cy.archive.ubuntu.com/ubuntu noble/main amd64 rpcbind amd64 1.2.6-7ubuntu2 [46.5 kB]
Get:3 http://cy.archive.ubuntu.com/ubuntu noble/main amd64 keyutils amd64 1.6.3-3build1 [56.8 kB]
Get:4 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 nfs-common amd64 1:2.6.4-3ubuntu5.1 [248 kB]
Fetched 400 kB in 2s (249 kB/s)
Selecting previously unselected package libnfsidmap1:amd64.
(Reading database ... 86650 files and directories currently installed.)
Preparing to unpack .../libnfsidmap1_1%3a2.6.4-3ubuntu5.1_amd64.deb ...
Unpacking libnfsidmap1:amd64 (1:2.6.4-3ubuntu5.1) ...
Selecting previously unselected package rpcbind.
Preparing to unpack .../rpcbind_1.2.6-7ubuntu2_amd64.deb ...
Unpacking rpcbind (1.2.6-7ubuntu2) ...
Selecting previously unselected package keyutils.
Preparing to unpack .../keyutils_1.6.3-3build1_amd64.deb ...
Unpacking keyutils (1.6.3-3build1) ...
Selecting previously unselected package nfs-common.
Preparing to unpack .../nfs-common_1%3a2.6.4-3ubuntu5.1_amd64.deb ...
Unpacking nfs-common (1:2.6.4-3ubuntu5.1) ...
Setting up libnfsidmap1:amd64 (1:2.6.4-3ubuntu5.1) ...
Setting up rpcbind (1.2.6-7ubuntu2) ...
Created symlink /etc/systemd/system/multi-user.target.wants/rpcbind.service → /usr/lib/systemd/system/rpcbind.service.
Created symlink /etc/systemd/system/sockets.target.wants/rpcbind.socket → /usr/lib/systemd/system/rpcbind.socket.
Setting up keyutils (1.6.3-3build1) ...
Setting up nfs-common (1:2.6.4-3ubuntu5.1) ...

Creating config file /etc/idmapd.conf with new version

Creating config file /etc/nfs.conf with new version
info: Selecting UID from range 100 to 999 ...

info: Adding system user `statd' (UID 111) ...
info: Adding new user `statd' (UID 111) with group `nogroup' ...
info: Not creating home directory `/var/lib/nfs'.
Created symlink /etc/systemd/system/multi-user.target.wants/nfs-client.target → /usr/lib/systemd/system/nfs-client.target.
Created symlink /etc/systemd/system/remote-fs.target.wants/nfs-client.target → /usr/lib/systemd/system/nfs-client.target.
auth-rpcgss-module.service is a disabled or a static unit, not starting it.
nfs-idmapd.service is a disabled or a static unit, not starting it.
nfs-utils.service is a disabled or a static unit, not starting it.
proc-fs-nfsd.mount is a disabled or a static unit, not starting it.
rpc-gssd.service is a disabled or a static unit, not starting it.
rpc-statd-notify.service is a disabled or a static unit, not starting it.
rpc-statd.service is a disabled or a static unit, not starting it.
rpc-svcgssd.service is a disabled or a static unit, not starting it.
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for libc-bin (2.39-0ubuntu8.4) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

root@otus:~# echo "192.168.63.187:/srv/share/ /mnt nfs vers=3,noauto,x-systemd.automount 0 0" >> /etc/fstab

root@otus:~# systemctl daemon-reload

root@otus:~# systemctl restart remote-fs.target

root@otus:~# mount | grep mnt
systemd-1 on /mnt type autofs (rw,relatime,fd=69,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=14695)

Проверка работоспособности 

root@otus2:~# cd /srv/share/upload/

root@otus2:/srv/share/upload# touch check_file


Заходим на клиент

root@otus:/# cd /mnt/upload

root@otus:/mnt/upload# touch client_file

root@otus:/mnt/upload# ls -l
total 0
-rw-r--r-- 1 root   root    0 Mar 29 19:35 check_file
-rw-r--r-- 1 nobody nogroup 0 Mar 29 19:42 client_file

Предварительно проверяем клиент:

root@otus:~# cd /mnt/upload

root@otus:/mnt/upload# ls -l
total 0
-rw-r--r-- 1 root   root    0 Mar 29 19:35 check_file
-rw-r--r-- 1 nobody nogroup 0 Mar 29 19:42 client_file

Проверяем сервер:

root@otus2:~# ls -l /srv/share/upload/
total 0
-rw-r--r-- 1 root   root    0 Mar 29 19:35 check_file
-rw-r--r-- 1 nobody nogroup 0 Mar 29 19:42 client_file

root@otus2:~# exportfs -s
/srv/share  192.168.63.44/32(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)

root@otus2:~# showmount -a 192.168.63.187
All mount points on 192.168.63.187:
192.168.63.44:/srv/share

Проверяем клиент:

dsavostyanov@otus:~$ showmount -a 192.168.63.187
All mount points on 192.168.63.187:
192.168.63.44:/srv/share

dsavostyanov@otus:~$ cd /mnt/upload

dsavostyanov@otus:/mnt/upload$ mount | grep mnt
systemd-1 on /mnt type autofs (rw,relatime,fd=66,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=4496)
192.168.63.187:/srv/share/ on /mnt type nfs (rw,relatime,vers=3,rsize=524288,wsize=524288,namlen=255,hard,proto=tcp,timeo=600,retrans=2,sec=sys,mountaddr=192.168.63.187,mountvers=3,mountport=51103,mountproto=udp,local_lock=none,addr=192.168.63.187)

dsavostyanov@otus:/mnt/upload$ ls -l
total 0
-rw-r--r-- 1 root   root    0 Mar 29 19:35 check_file
-rw-r--r-- 1 nobody nogroup 0 Mar 29 19:42 client_file

dsavostyanov@otus:/mnt/upload$ touch final_check

dsavostyanov@otus:/mnt/upload$ ls -l
total 0
-rw-r--r-- 1 root         root         0 Mar 29 19:35 check_file
-rw-r--r-- 1 nobody       nogroup      0 Mar 29 19:42 client_file
-rw-rw-r-- 1 dsavostyanov dsavostyanov 0 Mar 29 19:54 final_check

```
