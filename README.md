# Дисковая подсистема
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

dsavostyanov@ubuntu-otus:~$ sudo mdadm --zero-superblock --force /dev/sd{b,c,d}
mdadm: Unrecognised md component device - /dev/sdb
mdadm: Unrecognised md component device - /dev/sdc
mdadm: Unrecognised md component device - /dev/sdd
dsavostyanov@ubuntu-otus:~$ sudo mdadm --create --verbose /dev/md0 -l 5 -n 3 /dev/sd{b,c,d}
mdadm: layout defaults to left-symmetric
mdadm: layout defaults to left-symmetric
mdadm: chunk size defaults to 512K
mdadm: size set to 1046528K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
dsavostyanov@ubuntu-otus:~$ cat /proc/mdstat
Personalities : [linear] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md0 : active raid5 sdd[3] sdc[1] sdb[0]
      2093056 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]

unused devices: <none>


dsavostyanov@ubuntu-otus:~$ sudo mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Wed Mar 12 20:51:35 2025
        Raid Level : raid5
        Array Size : 2093056 (2044.00 MiB 2143.29 MB)
     Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
      Raid Devices : 3
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Wed Mar 12 20:51:40 2025
             State : clean
    Active Devices : 3
   Working Devices : 3
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : ubuntu-otus:0  (local to host ubuntu-otus)
              UUID : 8e9d172f:d98235b4:881ba584:2094dcd0
            Events : 18

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       3       8       48        2      active sync   /dev/sdd
	   
dsavostyanov@ubuntu-otus:~$ sudo mdadm /dev/md0 --fail /dev/sdd
mdadm: set /dev/sdd faulty in /dev/md0
dsavostyanov@ubuntu-otus:~$ cat /proc/mdstat
Personalities : [linear] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md0 : active raid5 sdd[3](F) sdc[1] sdb[0]
      2093056 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/2] [UU_]

unused devices: <none>

dsavostyanov@ubuntu-otus:~$ sudo mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Wed Mar 12 20:51:35 2025
        Raid Level : raid5
        Array Size : 2093056 (2044.00 MiB 2143.29 MB)
     Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
      Raid Devices : 3
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Wed Mar 12 20:57:07 2025
             State : clean, degraded
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 1
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : ubuntu-otus:0  (local to host ubuntu-otus)
              UUID : 8e9d172f:d98235b4:881ba584:2094dcd0
            Events : 20

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       -       0        0        2      removed

       3       8       48        -      faulty   /dev/sdd


dsavostyanov@ubuntu-otus:~$ sudo mdadm /dev/md0 --remove /dev/sdd
mdadm: hot removed /dev/sdd from /dev/md0

dsavostyanov@ubuntu-otus:~$ sudo mdadm --examine /dev/sdd
/dev/sdd:
          Magic : a92b4efc
        Version : 1.2
    Feature Map : 0x0
     Array UUID : 8e9d172f:d98235b4:881ba584:2094dcd0
           Name : ubuntu-otus:0  (local to host ubuntu-otus)
  Creation Time : Wed Mar 12 20:51:35 2025
     Raid Level : raid5
   Raid Devices : 3

 Avail Dev Size : 2093056 sectors (1022.00 MiB 1071.64 MB)
     Array Size : 2093056 KiB (2044.00 MiB 2143.29 MB)
    Data Offset : 4096 sectors
   Super Offset : 8 sectors
   Unused Space : before=4016 sectors, after=0 sectors
          State : clean
    Device UUID : 864936d7:84e472b5:3d6d674b:a43b7696

    Update Time : Wed Mar 12 21:05:17 2025
  Bad Block Log : 512 entries available at offset 16 sectors
       Checksum : 3887233b - correct
         Events : 40

         Layout : left-symmetric
     Chunk Size : 512K

   Device Role : Active device 2
   Array State : AAA ('A' == active, '.' == missing, 'R' == replacing)


dsavostyanov@ubuntu-otus:~$ sudo mdadm --zero-superblock --force /dev/sdd

dsavostyanov@ubuntu-otus:~$ sudo mdadm --zero-superblock --force /dev/sdd
mdadm: Unrecognised md component device - /dev/sdd

dsavostyanov@ubuntu-otus:~$ cat /proc/mdstat
Personalities : [linear] [raid0] [raid1] [raid6] [raid5] [raid4] [raid10]
md0 : active raid5 sdd[3] sdc[1] sdb[0]
      2093056 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/3] [UUU]

unused devices: <none>
dsavostyanov@ubuntu-otus:~$ sudo mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Wed Mar 12 20:51:35 2025
        Raid Level : raid5
        Array Size : 2093056 (2044.00 MiB 2143.29 MB)
     Used Dev Size : 1046528 (1022.00 MiB 1071.64 MB)
      Raid Devices : 3
     Total Devices : 3
       Persistence : Superblock is persistent

       Update Time : Wed Mar 12 21:09:59 2025
             State : clean
    Active Devices : 3
   Working Devices : 3
    Failed Devices : 0
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : ubuntu-otus:0  (local to host ubuntu-otus)
              UUID : 8e9d172f:d98235b4:881ba584:2094dcd0
            Events : 62

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       3       8       48        2      active sync   /dev/sdd


dsavostyanov@ubuntu-otus:~$ sudo parted -s /dev/md0 mklabel gpt

dsavostyanov@ubuntu-otus:~$ sudo parted /dev/md0 mkpart primary ext4 0% 20%
Information: You may need to update /etc/fstab.

dsavostyanov@ubuntu-otus:~$ sudo parted /dev/md0 mkpart primary ext4 20% 40%
Information: You may need to update /etc/fstab.

dsavostyanov@ubuntu-otus:~$ sudo parted /dev/md0 mkpart primary ext4 40% 60%
Information: You may need to update /etc/fstab.

dsavostyanov@ubuntu-otus:~$ sudo parted /dev/md0 mkpart primary ext4 60% 80%
Information: You may need to update /etc/fstab.

dsavostyanov@ubuntu-otus:~$ sudo parted /dev/md0 mkpart primary ext4 80% 100%
Information: You may need to update /etc/fstab.

dsavostyanov@ubuntu-otus:~$ for i in $(seq 1 5); do sudo mkfs.ext4 /dev/md0p$i; done
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 104448 4k blocks and 104448 inodes
Filesystem UUID: ee655f79-2c84-4c1c-801f-60d3bddb1d78
Superblock backups stored on blocks:
        32768, 98304

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 104704 4k blocks and 104704 inodes
Filesystem UUID: 6b75a204-b709-4685-a2c9-de47e7f4397a
Superblock backups stored on blocks:
        32768, 98304

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 104448 4k blocks and 104448 inodes
Filesystem UUID: 30fc5aa7-aaca-4036-8256-aa0111cbe95e
Superblock backups stored on blocks:
        32768, 98304

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 104704 4k blocks and 104704 inodes
Filesystem UUID: e4d21032-917c-422a-a559-56e86876003f
Superblock backups stored on blocks:
        32768, 98304

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 104448 4k blocks and 104448 inodes
Filesystem UUID: 77ffa145-89f4-42df-831d-c6f95ccc0051
Superblock backups stored on blocks:
        32768, 98304

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done

dsavostyanov@ubuntu-otus:~$ sudo mkdir -p /raid/part{1,2,3,4,5}

dsavostyanov@ubuntu-otus:~$ for i in $(seq 1 5); do sudo mount /dev/md0p$i /raid/part$i; done

```
