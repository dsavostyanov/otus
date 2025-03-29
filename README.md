# Стенд ZFS
```
dsavostyanov@ubuntu-otus:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0   20G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.8G  0 part /boot
└─sda3                      8:3    0 18.2G  0 part
  └─ubuntu--vg-ubuntu--lv 252:0    0   10G  0 lvm  /
sdb                         8:16   0    1G  0 disk
sdc                         8:32   0    1G  0 disk
sdd                         8:48   0    1G  0 disk
sde                         8:64   0    1G  0 disk
sdf                         8:80   0    1G  0 disk
sdg                         8:96   0    1G  0 disk
sdh                         8:112  0    1G  0 disk
sdi                         8:128  0    1G  0 disk

dsavostyanov@ubuntu-otus:~$ sudo apt install zfsutils-linux
[sudo] password for dsavostyanov:
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libnvpair3linux libuutil3linux libzfs4linux libzpool5linux zfs-zed
Suggested packages:
  nfs-kernel-server samba-common-bin zfs-initramfs | zfs-dracut
The following NEW packages will be installed:
  libnvpair3linux libuutil3linux libzfs4linux libzpool5linux zfs-zed zfsutils-linux
0 upgraded, 6 newly installed, 0 to remove and 28 not upgraded.
Need to get 2,356 kB of archives.
After this operation, 7,393 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 libnvpair3linux amd64 2.2.2-0ubuntu9.1 [61.6 kB]
Get:2 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 libuutil3linux amd64 2.2.2-0ubuntu9.1 [52.7 kB]
Get:3 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 libzfs4linux amd64 2.2.2-0ubuntu9.1 [226 kB]
Get:4 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 libzpool5linux amd64 2.2.2-0ubuntu9.1 [1,397 kB]
Get:5 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 zfsutils-linux amd64 2.2.2-0ubuntu9.1 [551 kB]
Get:6 http://cy.archive.ubuntu.com/ubuntu noble-updates/main amd64 zfs-zed amd64 2.2.2-0ubuntu9.1 [67.9 kB]
Fetched 2,356 kB in 12s (198 kB/s)
Selecting previously unselected package libnvpair3linux.
(Reading database ... 86644 files and directories currently installed.)
Preparing to unpack .../0-libnvpair3linux_2.2.2-0ubuntu9.1_amd64.deb ...
Unpacking libnvpair3linux (2.2.2-0ubuntu9.1) ...
Selecting previously unselected package libuutil3linux.
Preparing to unpack .../1-libuutil3linux_2.2.2-0ubuntu9.1_amd64.deb ...
Unpacking libuutil3linux (2.2.2-0ubuntu9.1) ...
Selecting previously unselected package libzfs4linux.
Preparing to unpack .../2-libzfs4linux_2.2.2-0ubuntu9.1_amd64.deb ...
Unpacking libzfs4linux (2.2.2-0ubuntu9.1) ...
Selecting previously unselected package libzpool5linux.
Preparing to unpack .../3-libzpool5linux_2.2.2-0ubuntu9.1_amd64.deb ...
Unpacking libzpool5linux (2.2.2-0ubuntu9.1) ...
Selecting previously unselected package zfsutils-linux.
Preparing to unpack .../4-zfsutils-linux_2.2.2-0ubuntu9.1_amd64.deb ...
Unpacking zfsutils-linux (2.2.2-0ubuntu9.1) ...
Selecting previously unselected package zfs-zed.
Preparing to unpack .../5-zfs-zed_2.2.2-0ubuntu9.1_amd64.deb ...
Unpacking zfs-zed (2.2.2-0ubuntu9.1) ...
Setting up libnvpair3linux (2.2.2-0ubuntu9.1) ...
Setting up libuutil3linux (2.2.2-0ubuntu9.1) ...
Setting up libzpool5linux (2.2.2-0ubuntu9.1) ...
Setting up libzfs4linux (2.2.2-0ubuntu9.1) ...
Setting up zfsutils-linux (2.2.2-0ubuntu9.1) ...
insmod /lib/modules/6.8.0-54-generic/kernel/zfs/spl.ko.zst
insmod /lib/modules/6.8.0-54-generic/kernel/zfs/zfs.ko.zst
Created symlink /etc/systemd/system/zfs-import.target.wants/zfs-import-cache.service → /usr/lib/systemd/system/zfs-import-cache.service.
Created symlink /etc/systemd/system/zfs.target.wants/zfs-import.target → /usr/lib/systemd/system/zfs-import.target.
Created symlink /etc/systemd/system/zfs-mount.service.wants/zfs-load-module.service → /usr/lib/systemd/system/zfs-load-module.service.
Created symlink /etc/systemd/system/zfs.target.wants/zfs-load-module.service → /usr/lib/systemd/system/zfs-load-module.service.
Created symlink /etc/systemd/system/zfs.target.wants/zfs-mount.service → /usr/lib/systemd/system/zfs-mount.service.
Created symlink /etc/systemd/system/zfs.target.wants/zfs-share.service → /usr/lib/systemd/system/zfs-share.service.
Created symlink /etc/systemd/system/zfs-volumes.target.wants/zfs-volume-wait.service → /usr/lib/systemd/system/zfs-volume-wait.service.
Created symlink /etc/systemd/system/zfs.target.wants/zfs-volumes.target → /usr/lib/systemd/system/zfs-volumes.target.
Created symlink /etc/systemd/system/multi-user.target.wants/zfs.target → /usr/lib/systemd/system/zfs.target.
zfs-import-scan.service is a disabled or a static unit, not starting it.
Setting up zfs-zed (2.2.2-0ubuntu9.1) ...
Created symlink /etc/systemd/system/zed.service → /usr/lib/systemd/system/zfs-zed.service.
Created symlink /etc/systemd/system/zfs.target.wants/zfs-zed.service → /usr/lib/systemd/system/zfs-zed.service.
Processing triggers for man-db (2.12.0-4build2) ...
Processing triggers for libc-bin (2.39-0ubuntu8.4) ...
Scanning processes...
Scanning linux images...

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

dsavostyanov@ubuntu-otus:~$ sudo zpool create otus1 mirror /dev/sdb /dev/sdc

dsavostyanov@ubuntu-otus:~$ sudo zpool create otus2 mirror /dev/sdd /dev/sde

dsavostyanov@ubuntu-otus:~$ sudo zpool create otus3 mirror /dev/sdf /dev/sdg

dsavostyanov@ubuntu-otus:~$ sudo zpool create otus4 mirror /dev/sdh /dev/sdi

dsavostyanov@ubuntu-otus:~$ zpool list
NAME    SIZE  ALLOC   FREE  CKPOINT  EXPANDSZ   FRAG    CAP  DEDUP    HEALTH  ALTROOT
otus1   960M   111K   960M        -         -     0%     0%  1.00x    ONLINE  -
otus2   960M   112K   960M        -         -     0%     0%  1.00x    ONLINE  -
otus3   960M   114K   960M        -         -     0%     0%  1.00x    ONLINE  -
otus4   960M   138K   960M        -         -     0%     0%  1.00x    ONLINE  -

root@ubuntu-otus:~# zfs set compression=lzjb otus1

root@ubuntu-otus:~# zfs set compression=lz4 otus2

root@ubuntu-otus:~# zfs set compression=gzip-9 otus3

root@ubuntu-otus:~# zfs set compression=zle otus4
	
root@ubuntu-otus:~# zfs get all | grep compression
otus1  compression           lzjb                   local
otus2  compression           lz4                    local
otus3  compression           gzip-9                 local
otus4  compression           zle                    local

root@ubuntu-otus:~# for i in {1..4}; do wget -P /otus$i https://gutenberg.org/cache/epub/2600/pg2600.converter.log; done

root@ubuntu-otus:~# ls -l /otus*
/otus1:
total 22098
-rw-r--r-- 1 root root 41130189 Mar  2 08:31 pg2600.converter.log

/otus2:
total 18007
-rw-r--r-- 1 root root 41130189 Mar 29 17:27 pg2600.converter.log

/otus3:
total 10966
-rw-r--r-- 1 root root 41130189 Mar 29 17:28 pg2600.converter.log

/otus4:
total 40195
-rw-r--r-- 1 root root 41130189 Mar 29 17:28 pg2600.converter.log

root@ubuntu-otus:~# zfs list
NAME    USED  AVAIL  REFER  MOUNTPOINT
otus1  21.7M   810M  21.6M  /otus1
otus2  17.7M   814M  17.6M  /otus2
otus3  10.9M   821M  10.7M  /otus3
otus4  39.4M   793M  39.3M  /otus4

root@ubuntu-otus:~# zfs get all | grep compressratio | grep -v ref
otus1  compressratio         1.81x                  -
otus2  compressratio         2.23x                  -
otus3  compressratio         3.65x                  -
otus4  compressratio         1.00x                  -

root@ubuntu-otus:~# wget -O archive.tar.gz --no-check-certificate 'https://drive.usercontent.google.com/download?id=1MvrcEp-WgAQe57aDEzxSRalPAwbNN1Bb&export=download'
--2025-03-29 17:32:15--  https://drive.usercontent.google.com/download?id=1MvrcEp-WgAQe57aDEzxSRalPAwbNN1Bb&export=download
Resolving drive.usercontent.google.com (drive.usercontent.google.com)... 216.58.212.97, 2a00:1450:4006:80b::2001
Connecting to drive.usercontent.google.com (drive.usercontent.google.com)|216.58.212.97|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7275140 (6.9M) [application/octet-stream]
Saving to: ‘archive.tar.gz’

archive.tar.gz                                  100%[=====================================================================================================>]   6.94M  90.8KB/s    in 77s

2025-03-29 17:33:44 (92.2 KB/s) - ‘archive.tar.gz’ saved [7275140/7275140]

root@ubuntu-otus:~# tar -xzvf archive.tar.gz
zpoolexport/
zpoolexport/filea
zpoolexport/fileb

root@ubuntu-otus:~#  zpool import -d zpoolexport/
   pool: otus
     id: 6554193320433390805
  state: ONLINE
status: Some supported features are not enabled on the pool.
        (Note that they may be intentionally disabled if the
        'compatibility' property is set.)
 action: The pool can be imported using its name or numeric identifier, though
        some features will not be available without an explicit 'zpool upgrade'.
 config:

        otus                         ONLINE
          mirror-0                   ONLINE
            /root/zpoolexport/filea  ONLINE
            /root/zpoolexport/fileb  ONLINE

root@ubuntu-otus:~# zpool import -d zpoolexport/ otus

root@ubuntu-otus:~# zpool status
  pool: otus
 state: ONLINE
status: Some supported and requested features are not enabled on the pool.
        The pool can still be used, but some features are unavailable.
action: Enable all features using 'zpool upgrade'. Once this is done,
        the pool may no longer be accessible by software that does not support
        the features. See zpool-features(7) for details.
config:

        NAME                         STATE     READ WRITE CKSUM
        otus                         ONLINE       0     0     0
          mirror-0                   ONLINE       0     0     0
            /root/zpoolexport/filea  ONLINE       0     0     0
            /root/zpoolexport/fileb  ONLINE       0     0     0

errors: No known data errors

  pool: otus1
 state: ONLINE
config:

        NAME        STATE     READ WRITE CKSUM
        otus1       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdb     ONLINE       0     0     0
            sdc     ONLINE       0     0     0

errors: No known data errors

  pool: otus2
 state: ONLINE
config:

        NAME        STATE     READ WRITE CKSUM
        otus2       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdd     ONLINE       0     0     0
            sde     ONLINE       0     0     0

errors: No known data errors

  pool: otus3
 state: ONLINE
config:

        NAME        STATE     READ WRITE CKSUM
        otus3       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdf     ONLINE       0     0     0
            sdg     ONLINE       0     0     0

errors: No known data errors

  pool: otus4
 state: ONLINE
config:

        NAME        STATE     READ WRITE CKSUM
        otus4       ONLINE       0     0     0
          mirror-0  ONLINE       0     0     0
            sdh     ONLINE       0     0     0
            sdi     ONLINE       0     0     0

errors: No known data errors

root@ubuntu-otus:~# zfs get all otus
NAME  PROPERTY              VALUE                  SOURCE
otus  type                  filesystem             -
otus  creation              Fri May 15  4:00 2020  -
otus  used                  2.04M                  -
otus  available             350M                   -
otus  referenced            24K                    -
otus  compressratio         1.00x                  -
otus  mounted               yes                    -
otus  quota                 none                   default
otus  reservation           none                   default
otus  recordsize            128K                   local
otus  mountpoint            /otus                  default
otus  sharenfs              off                    default
otus  checksum              sha256                 local
otus  compression           zle                    local
otus  atime                 on                     default
otus  devices               on                     default
otus  exec                  on                     default
otus  setuid                on                     default
otus  readonly              off                    default
otus  zoned                 off                    default
otus  snapdir               hidden                 default
otus  aclmode               discard                default
otus  aclinherit            restricted             default
otus  createtxg             1                      -
otus  canmount              on                     default
otus  xattr                 on                     default
otus  copies                1                      default
otus  version               5                      -
otus  utf8only              off                    -
otus  normalization         none                   -
otus  casesensitivity       sensitive              -
otus  vscan                 off                    default
otus  nbmand                off                    default
otus  sharesmb              off                    default
otus  refquota              none                   default
otus  refreservation        none                   default
otus  guid                  14592242904030363272   -
otus  primarycache          all                    default
otus  secondarycache        all                    default
otus  usedbysnapshots       0B                     -
otus  usedbydataset         24K                    -
otus  usedbychildren        2.01M                  -
otus  usedbyrefreservation  0B                     -
otus  logbias               latency                default
otus  objsetid              54                     -
otus  dedup                 off                    default
otus  mlslabel              none                   default
otus  sync                  standard               default
otus  dnodesize             legacy                 default
otus  refcompressratio      1.00x                  -
otus  written               24K                    -
otus  logicalused           1020K                  -
otus  logicalreferenced     12K                    -
otus  volmode               default                default
otus  filesystem_limit      none                   default
otus  snapshot_limit        none                   default
otus  filesystem_count      none                   default
otus  snapshot_count        none                   default
otus  snapdev               hidden                 default
otus  acltype               off                    default
otus  context               none                   default
otus  fscontext             none                   default
otus  defcontext            none                   default
otus  rootcontext           none                   default
otus  relatime              on                     default
otus  redundant_metadata    all                    default
otus  overlay               on                     default
otus  encryption            off                    default
otus  keylocation           none                   default
otus  keyformat             none                   default
otus  pbkdf2iters           0                      default
otus  special_small_blocks  0                      default

root@ubuntu-otus:~# zfs get available otus
NAME  PROPERTY   VALUE  SOURCE
otus  available  350M   -

root@ubuntu-otus:~# zfs get readonly otus
NAME  PROPERTY  VALUE   SOURCE
otus  readonly  off     default

root@ubuntu-otus:~# zfs get recordsize otus
NAME  PROPERTY    VALUE    SOURCE
otus  recordsize  128K     local

root@ubuntu-otus:~# zfs get compression otus
NAME  PROPERTY     VALUE           SOURCE
otus  compression  zle             local

root@ubuntu-otus:~# zfs get checksum otus
NAME  PROPERTY  VALUE      SOURCE
otus  checksum  sha256     local

root@ubuntu-otus:~# wget -O otus_task2.file --no-check-certificate https://drive.usercontent.google.com/download?id=1wgxjih8YZ-cqLqaZVa0lA3h3Y029c3oI&export=download

root@ubuntu-otus:~# zfs receive otus/test@today < otus_task2.file
[2]+  Done                    wget -O otus_task2.file --no-check-certificate https://drive.usercontent.google.com/download?id=1wgxjih8YZ-cqLqaZVa0lA3h3Y029c3oI

root@ubuntu-otus:~# find /otus/test -name "secret_message"
/otus/test/task1/file_mess/secret_message

root@ubuntu-otus:~# cat /otus/test/task1/file_mess/secret_message
https://otus.ru/lessons/linux-hl/
```
