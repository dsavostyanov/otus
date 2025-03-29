# LVM - начало работы
```
dsavostyanov@ubuntu-otus:~$ sudo -i

root@ubuntu-otus:~# lsblk
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

root@ubuntu-otus:~# pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
  
root@ubuntu-otus:~# vgcreate otus /dev/sdb
  Volume group "otus" successfully created
  
root@ubuntu-otus:~# lvcreate -l+80%FREE -n test otus
  Logical volume "test" created.

root@ubuntu-otus:~# vgdisplay otus
  --- Volume group ---
  VG Name               otus
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               1020.00 MiB
  PE Size               4.00 MiB
  Total PE              255
  Alloc PE / Size       204 / 816.00 MiB
  Free  PE / Size       51 / 204.00 MiB
  VG UUID               Z8Iar7-1Lqa-qMXL-Sufe-YM6b-Z2ed-JNn0q2

root@ubuntu-otus:~# vgdisplay -v otus | grep 'PV Name'
  PV Name               /dev/sdb

root@ubuntu-otus:~# lvdisplay /dev/otus/test
  --- Logical volume ---
  LV Path                /dev/otus/test
  LV Name                test
  VG Name                otus
  LV UUID                fWosAN-VPbq-3MDK-HN6o-9Xsf-rt3I-5WRUXu
  LV Write Access        read/write
  LV Creation host, time ubuntu-otus, 2025-03-23 11:05:08 +0000
  LV Status              available
  # open                 0
  LV Size                816.00 MiB
  Current LE             204
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           252:1

root@ubuntu-otus:~# vgs; lvs
  VG        #PV #LV #SN Attr   VSize    VFree
  otus        1   1   0 wz--n- 1020.00m 204.00m
  ubuntu-vg   1   1   0 wz--n-   18.22g   8.22g
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  test      otus      -wi-a----- 816.00m
  ubuntu-lv ubuntu-vg -wi-ao----  10.00g


root@ubuntu-otus:~# lvcreate -L100M -n small otus
  Logical volume "small" created.
  
root@ubuntu-otus:~# lvs
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  small     otus      -wi-a----- 100.00m
  test      otus      -wi-a----- 816.00m
  ubuntu-lv ubuntu-vg -wi-ao----  10.00g
  
root@ubuntu-otus:~# mkfs.ext4 /dev/otus/test
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 208896 4k blocks and 52304 inodes
Filesystem UUID: 72d13e14-638a-49cd-ace4-84d86ae654f1
Superblock backups stored on blocks:
        32768, 98304, 163840

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

root@ubuntu-otus:~# mkdir /data

root@ubuntu-otus:~# mount /dev/otus/test /data/

root@ubuntu-otus:~# mount | grep /data
/dev/mapper/otus-test on /data type ext4 (rw,relatime)

Расширение LVM

root@ubuntu-otus:~# pvcreate /dev/sdc
  Physical volume "/dev/sdc" successfully created.
  
root@ubuntu-otus:~# vgextend otus /dev/sdc
  Volume group "otus" successfully extended
  
root@ubuntu-otus:~# vgdisplay -v otus | grep 'PV Name'
  PV Name               /dev/sdb
  PV Name               /dev/sdc
  
root@ubuntu-otus:~#  vgs
  VG        #PV #LV #SN Attr   VSize  VFree
  otus        2   2   0 wz--n-  1.99g <1.10g
  ubuntu-vg   1   1   0 wz--n- 18.22g  8.22g


root@ubuntu-otus:~# dd if=/dev/zero of=/data/test.log bs=1M count=8000 status=progress
761266176 bytes (761 MB, 726 MiB) copied, 13 s, 58.5 MB/s
dd: error writing '/data/test.log': No space left on device
770+0 records in
769+0 records out
806920192 bytes (807 MB, 770 MiB) copied, 14.0053 s, 57.6 MB/s

root@ubuntu-otus:~# df -Th /data/
Filesystem            Type  Size  Used Avail Use% Mounted on
/dev/mapper/otus-test ext4  786M  770M     0 100% /data

root@ubuntu-otus:~# lvextend -l+80%FREE /dev/otus/test
  Size of logical volume otus/test changed from 816.00 MiB (204 extents) to <1.68 GiB (429 extents).
  Logical volume otus/test successfully resized.
  
root@ubuntu-otus:~# lvs /dev/otus/test
  LV   VG   Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  test otus -wi-ao---- <1.68g

root@ubuntu-otus:~# df -Th /data
Filesystem            Type  Size  Used Avail Use% Mounted on
/dev/mapper/otus-test ext4  786M  770M     0 100% /data

root@ubuntu-otus:~# resize2fs /dev/otus/test
resize2fs 1.47.0 (5-Feb-2023)
Filesystem at /dev/otus/test is mounted on /data; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 1
The filesystem on /dev/otus/test is now 439296 (4k) blocks long.

root@ubuntu-otus:~# df -Th /data
Filesystem            Type  Size  Used Avail Use% Mounted on
/dev/mapper/otus-test ext4  1.7G  770M  813M  49% /data

root@ubuntu-otus:/# umount /data/

root@ubuntu-otus:/# e2fsck -fy /dev/otus/test
e2fsck 1.47.0 (5-Feb-2023)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/otus/test: 12/104608 files (0.0% non-contiguous), 208289/439296 blocks

root@ubuntu-otus:/# resize2fs /dev/otus/test 1500M
resize2fs 1.47.0 (5-Feb-2023)
Resizing the filesystem on /dev/otus/test to 384000 (4k) blocks.
The filesystem on /dev/otus/test is now 384000 (4k) blocks long.

root@ubuntu-otus:/# lvreduce /dev/otus/test -L 1500M
  WARNING: Reducing active logical volume to 1.46 GiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce otus/test? [y/n]: y
  Size of logical volume otus/test changed from 1.68 GiB (431 extents) to 1.46 GiB (375 extents).
  Logical volume otus/test successfully resized.
  
root@ubuntu-otus:/# mount /dev/otus/test /data/

root@ubuntu-otus:/# df -Th /data/
Filesystem            Type  Size  Used Avail Use% Mounted on
/dev/mapper/otus-test ext4  1.5G  770M  610M  56% /data

root@ubuntu-otus:/# lvs /dev/otus/test
  LV   VG   Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  test otus -wi-ao---- 1.46g

Работа со снапшотами

root@ubuntu-otus:/# lvcreate -L 300M -s -n test-snap /dev/otus/test
  Logical volume "test-snap" created.

root@ubuntu-otus:/# vgs -o +lv_size,lv_name | grep test
  otus        2   3   1 wz--n-  1.99g 140.00m   1.46g test
  otus        2   3   1 wz--n-  1.99g 140.00m 300.00m test-snap

root@ubuntu-otus:/# lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0   20G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.8G  0 part /boot
└─sda3                      8:3    0 18.2G  0 part
  └─ubuntu--vg-ubuntu--lv 252:0    0   10G  0 lvm  /
sdb                         8:16   0    1G  0 disk
├─otus-small              252:2    0  100M  0 lvm
└─otus-test-real          252:3    0  1.5G  0 lvm
  ├─otus-test             252:1    0  1.5G  0 lvm  /data
  └─otus-test--snap       252:5    0  1.5G  0 lvm
sdc                         8:32   0    1G  0 disk
├─otus-test-real          252:3    0  1.5G  0 lvm
│ ├─otus-test             252:1    0  1.5G  0 lvm  /data
│ └─otus-test--snap       252:5    0  1.5G  0 lvm
└─otus-test--snap-cow     252:4    0  300M  0 lvm
  └─otus-test--snap       252:5    0  1.5G  0 lvm
sdd                         8:48   0    1G  0 disk
sde                         8:64   0    1G  0 disk

root@ubuntu-otus:/# mkdir /data-snap

root@ubuntu-otus:/# mount /dev/otus/test-snap /data-snap/

root@ubuntu-otus:/# ll /data-snap/
total 788036
drwxr-xr-x  3 root root      4096 Mar 23 11:14 ./
drwxr-xr-x 26 root root      4096 Mar 23 15:41 ../
drwx------  2 root root     16384 Mar 23 11:09 lost+found/
-rw-r--r--  1 root root 806920192 Mar 23 11:14 test.log

root@ubuntu-otus:/# umount /data-snap

root@ubuntu-otus:/# rm /data/test.log

root@ubuntu-otus:/# ll /data
total 24
drwxr-xr-x  3 root root  4096 Mar 23 15:43 ./
drwxr-xr-x 26 root root  4096 Mar 23 15:41 ../
drwx------  2 root root 16384 Mar 23 11:09 lost+found/
root@ubuntu-otus:/# umount /data
root@ubuntu-otus:/# lvconvert --merge /dev/otus/test-snap
  Merging of volume otus/test-snap started.
  otus/test: Merged: 100.00%

root@ubuntu-otus:/# mount /dev/otus/test /data

root@ubuntu-otus:/# ll /data
total 788036
drwxr-xr-x  3 root root      4096 Mar 23 11:14 ./
drwxr-xr-x 26 root root      4096 Mar 23 15:41 ../
drwx------  2 root root     16384 Mar 23 11:09 lost+found/
-rw-r--r--  1 root root 806920192 Mar 23 11:14 test.log

Работа с LVM-RAID

root@ubuntu-otus:/# pvcreate /dev/sd{d,e}
  Physical volume "/dev/sdd" successfully created.
  Physical volume "/dev/sde" successfully created.
  
root@ubuntu-otus:/# vgcreate vg0 /dev/sd{d,e}
  Volume group "vg0" successfully created
  
root@ubuntu-otus:/# lvcreate -l+80%FREE -m1 -n mirror vg0
  Logical volume "mirror" created.
  
root@ubuntu-otus:/# lvs
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  small     otus      -wi-a----- 100.00m
  test      otus      -wi-ao----   1.46g
  ubuntu-lv ubuntu-vg -wi-ao----  10.00g
  mirror    vg0       rwi-a-r--- 816.00m                                    100.00

Уменьшить том под / до 8G

root@ubuntu-otus:/# pvcreate /dev/sdf
  Physical volume "/dev/sdf" successfully created.
  
root@ubuntu-otus:/# vgcreate vg_root /dev/sdf
  Volume group "vg_root" successfully created
  
root@ubuntu-otus:/# lvcreate -n lv_root -l +100%FREE /dev/vg_root
  Logical volume "lv_root" created.
  
root@ubuntu-otus:/# mkfs.ext4 /dev/vg_root/lv_root
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 2620416 4k blocks and 655360 inodes
Filesystem UUID: 5c0f7a94-14fb-494e-8433-cdddf4e6d16e
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

root@ubuntu-otus:/# mount /dev/vg_root/lv_root /mnt

root@ubuntu-otus:/# rsync -avxHAX --progress / /mnt/

root@ubuntu-otus:/# for i in /proc/ /sys/ /dev/ /run/ /boot/; \
 do mount --bind $i /mnt/$i; done
 
root@ubuntu-otus:/# chroot /mnt/

root@ubuntu-otus:/# grub-mkconfig -o /boot/grub/grub.cfg
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.13.5-061305-generic
Found initrd image: /boot/initrd.img-6.13.5-061305-generic
Found linux image: /boot/vmlinuz-6.8.0-55-generic
Found initrd image: /boot/initrd.img-6.8.0-55-generic
Found linux image: /boot/vmlinuz-6.8.0-54-generic
Found initrd image: /boot/initrd.img-6.8.0-54-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done

root@ubuntu-otus:/# update-initramfs -u
update-initramfs: Generating /boot/initrd.img-6.13.5-061305-generic


dsavostyanov@ubuntu-otus:~$ sudo reboot
[sudo] password for dsavostyanov:

Broadcast message from root@ubuntu-otus on pts/1 (Sun 2025-03-23 16:12:18 UTC):

The system will reboot now!

dsavostyanov@ubuntu-otus:~$ lsblk
NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda                         8:0    0   20G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0  1.8G  0 part /boot
└─sda3                      8:3    0 18.2G  0 part
  └─ubuntu--vg-ubuntu--lv 252:3    0   10G  0 lvm
sdb                         8:16   0    1G  0 disk
├─otus-test               252:0    0  1.5G  0 lvm
└─otus-small              252:1    0  100M  0 lvm
sdc                         8:32   0    1G  0 disk
└─otus-test               252:0    0  1.5G  0 lvm
sdd                         8:48   0    1G  0 disk
├─vg0-mirror_rmeta_0      252:4    0    4M  0 lvm
│ └─vg0-mirror            252:8    0  816M  0 lvm
└─vg0-mirror_rimage_0     252:5    0  816M  0 lvm
  └─vg0-mirror            252:8    0  816M  0 lvm
sde                         8:64   0    1G  0 disk
├─vg0-mirror_rmeta_1      252:6    0    4M  0 lvm
│ └─vg0-mirror            252:8    0  816M  0 lvm
└─vg0-mirror_rimage_1     252:7    0  816M  0 lvm
  └─vg0-mirror            252:8    0  816M  0 lvm
sdf                         8:80   0   10G  0 disk
└─vg_root-lv_root         252:2    0   10G  0 lvm  /

dsavostyanov@ubuntu-otus:~$ sudo lvremove /dev/ubuntu-vg/ubuntu-lv
[sudo] password for dsavostyanov:
Do you really want to remove and DISCARD active logical volume ubuntu-vg/ubuntu-lv? [y/n]: y
  Logical volume "ubuntu-lv" successfully removed.
dsavostyanov@ubuntu-otus:~$

root@ubuntu-otus:~# lvcreate -n ubuntu-vg/ubuntu-lv -L 8G /dev/ubuntu-vg
WARNING: ext4 signature detected on /dev/ubuntu-vg/ubuntu-lv at offset 1080. Wipe it? [y/n]: y
  Wiping ext4 signature on /dev/ubuntu-vg/ubuntu-lv.
  Logical volume "ubuntu-lv" created.
  
root@ubuntu-otus:~# mkfs.ext4 /dev/ubuntu-vg/ubuntu-lv
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 2097152 4k blocks and 524288 inodes
Filesystem UUID: 6ca2f19e-1d9f-472e-bada-22db6b52c63c
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

root@ubuntu-otus:~# mount /dev/ubuntu-vg/ubuntu-lv /mnt

root@ubuntu-otus:~# rsync -avxHAX --progress / /mnt/

root@ubuntu-otus:~# for i in /proc/ /sys/ /dev/ /run/ /boot/; \
 do mount --bind $i /mnt/$i; done

root@ubuntu-otus:/# grub-mkconfig -o /boot/grub/grub.cfg
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.13.5-061305-generic
Found initrd image: /boot/initrd.img-6.13.5-061305-generic
Found linux image: /boot/vmlinuz-6.8.0-55-generic
Found initrd image: /boot/initrd.img-6.8.0-55-generic
Found linux image: /boot/vmlinuz-6.8.0-54-generic
Found initrd image: /boot/initrd.img-6.8.0-54-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done

root@ubuntu-otus:/# update-initramfs -u
update-initramfs: Generating /boot/initrd.img-6.13.5-061305-generic
W: Couldn't identify type of root file system for fsck hook

root@ubuntu-otus:/# pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.

root@ubuntu-otus:/# vgcreate vg_var /dev/sdb /dev/sdc
  Volume group "vg_var" successfully created

root@ubuntu-otus:/# lvcreate -L 950M -m1 -n lv_var vg_var
  Rounding up size to full physical extent 952.00 MiB
  Logical volume "lv_var" created.

root@ubuntu-otus:/# lvcreate -L 950M -m1 -n lv_var vg_var
  Rounding up size to full physical extent 952.00 MiB
  Logical volume "lv_var" created.
root@ubuntu-otus:/#
root@ubuntu-otus:/# mkfs.ext4 /dev/vg_var/lv_var
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 243712 4k blocks and 60928 inodes
Filesystem UUID: 270d65ea-48ae-4973-ada7-8da30d34ab25
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

root@ubuntu-otus:/# mount /dev/vg_var/lv_var /mnt

root@ubuntu-otus:/# cp -aR /var/* /mnt/

root@ubuntu-otus:/# mkdir /tmp/oldvar && mv /var/* /tmp/oldvar

root@ubuntu-otus:/# umount /mnt

root@ubuntu-otus:/# mount /dev/vg_var/lv_var /var

root@ubuntu-otus:/# echo "`blkid | grep var: | awk '{print $2}'` \
 /var ext4 defaults 0 0" >> /etc/fstab
 
root@ubuntu-otus:~# lvremove /dev/vg_root/lv_root
Do you really want to remove and DISCARD active logical volume vg_root/lv_root? [y/n]: y
  Logical volume "lv_root" successfully removed.
  
 root@ubuntu-otus:~# vgremove /dev/vg_root
  Volume group "vg_root" successfully removed
  
root@ubuntu-otus:~# pvremove /dev/sdf
  Labels on physical volume "/dev/sdf" successfully wiped.
  
root@ubuntu-otus:~# lvcreate -n LogVol_Home -L 2G /dev/ubuntu-vg
  Logical volume "LogVol_Home" created.
  
root@ubuntu-otus:~# mkfs.ext4 /dev/ubuntu-vg/LogVol_Home
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 524288 4k blocks and 131072 inodes
Filesystem UUID: 20fd207a-6403-4482-9a5b-d04124e336c6
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

root@ubuntu-otus:~# mount /dev/ubuntu-vg/LogVol_Home /mnt/

root@ubuntu-otus:~# cp -aR /home/* /mnt/

root@ubuntu-otus:~# rm -rf /home/*

root@ubuntu-otus:~# umount /mnt


root@ubuntu-otus:~# echo "`blkid | grep Home | awk '{print $2}'` \
 /home xfs defaults 0 0" >> /etc/fstab
 
root@ubuntu-otus:~# touch /home/file{1..20}

root@ubuntu-otus:~# lvcreate -L 100MB -s -n home_snap \
 /dev/ubuntu-vg/LogVol_Home
  Logical volume "home_snap" created.

root@ubuntu-otus:~# rm -f /home/file{11..20}

root@ubuntu-otus:~# umount /home

root@ubuntu-otus:~# lvconvert --merge /dev/ubuntu-vg/home_snap
  Merging of volume ubuntu-vg/home_snap started.
  ubuntu-vg/LogVol_Home: Merged: 100.00%

root@ubuntu-otus:~# mount /dev/mapper/ubuntu--vg-LogVol_Home /home

root@ubuntu-otus:~# ls -al /home
total 28
drwxr-xr-x  4 root         root          4096 Mar 23 16:53 .
drwxr-xr-x 26 root         root          4096 Mar 23 15:41 ..
drwxr-x---  5 dsavostyanov dsavostyanov  4096 Mar 12 21:42 dsavostyanov
-rw-r--r--  1 root         root             0 Mar 23 16:53 file1
-rw-r--r--  1 root         root             0 Mar 23 16:53 file10
-rw-r--r--  1 root         root             0 Mar 23 16:53 file11
-rw-r--r--  1 root         root             0 Mar 23 16:53 file12
-rw-r--r--  1 root         root             0 Mar 23 16:53 file13
-rw-r--r--  1 root         root             0 Mar 23 16:53 file14
-rw-r--r--  1 root         root             0 Mar 23 16:53 file15
-rw-r--r--  1 root         root             0 Mar 23 16:53 file16
-rw-r--r--  1 root         root             0 Mar 23 16:53 file17
-rw-r--r--  1 root         root             0 Mar 23 16:53 file18
-rw-r--r--  1 root         root             0 Mar 23 16:53 file19
-rw-r--r--  1 root         root             0 Mar 23 16:53 file2
-rw-r--r--  1 root         root             0 Mar 23 16:53 file20
-rw-r--r--  1 root         root             0 Mar 23 16:53 file3
-rw-r--r--  1 root         root             0 Mar 23 16:53 file4
-rw-r--r--  1 root         root             0 Mar 23 16:53 file5
-rw-r--r--  1 root         root             0 Mar 23 16:53 file6
-rw-r--r--  1 root         root             0 Mar 23 16:53 file7
-rw-r--r--  1 root         root             0 Mar 23 16:53 file8
-rw-r--r--  1 root         root             0 Mar 23 16:53 file9
drwx------  2 root         root         16384 Mar 23 16:51 lost+found
```
