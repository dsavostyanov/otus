# Обновление ядра системы
```
dsavostyanov@ubuntu-otus:~$ uname -r
6.8.0-54-generic

dsavostyanov@ubuntu-otus:~$ mkdir kernel && cd kernel

dsavostyanov@ubuntu-otus:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-headers-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
--2025-03-02 13:49:29--  https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-headers-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.74, 185.125.189.76, 185.125.189.75
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.74|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3827358 (3.6M) [application/x-debian-package]
Saving to: ‘linux-headers-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb’

linux-headers-6.13.5-061305-generic_6.13.5-0613 100%[=====================================================================================================>]   3.65M  2.65MB/s    in 1.4s

2025-03-02 13:49:34 (2.65 MB/s) - ‘linux-headers-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb’ saved [3827358/3827358]

dsavostyanov@ubuntu-otus:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-headers-6.13.5-061305_6.13.5-061305.202502271338_all.deb
--2025-03-02 13:49:46--  https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-headers-6.13.5-061305_6.13.5-061305.202502271338_all.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.74, 185.125.189.75, 185.125.189.76
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.74|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13885596 (13M) [application/x-debian-package]
Saving to: ‘linux-headers-6.13.5-061305_6.13.5-061305.202502271338_all.deb’

linux-headers-6.13.5-061305_6.13.5-061305.20250 100%[=====================================================================================================>]  13.24M  5.11MB/s    in 2.6s

2025-03-02 13:49:50 (5.11 MB/s) - ‘linux-headers-6.13.5-061305_6.13.5-061305.202502271338_all.deb’ saved [13885596/13885596]

dsavostyanov@ubuntu-otus:~/kernel$ https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
-bash: https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb: No such file or directory
dsavostyanov@ubuntu-otus:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
--2025-03-02 13:50:39--  https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.75, 185.125.189.76, 185.125.189.74
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.75|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 15882432 (15M) [application/x-debian-package]
Saving to: ‘linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb’

linux-image-unsigned-6.13.5-061305-generic_6.13 100%[=====================================================================================================>]  15.15M  3.00MB/s    in 5.1s

2025-03-02 13:50:45 (2.97 MB/s) - ‘linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb’ saved [15882432/15882432]

dsavostyanov@ubuntu-otus:~/kernel$ wget https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-modules-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
--2025-03-02 13:51:16--  https://kernel.ubuntu.com/mainline/v6.13.5/amd64/linux-modules-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.76, 185.125.189.75, 185.125.189.74
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.76|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 191344832 (182M) [application/x-debian-package]
Saving to: ‘linux-modules-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb’

linux-modules-6.13.5-061305-generic_6.13.5-0613 100%[=====================================================================================================>] 182.48M  2.65MB/s    in 52s

2025-03-02 13:52:09 (3.49 MB/s) - ‘linux-modules-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb’ saved [191344832/191344832]

dsavostyanov@ubuntu-otus:~/kernel$ sudo dpkg -i *.deb
[sudo] password for dsavostyanov:
Selecting previously unselected package linux-headers-6.13.5-061305.
(Reading database ... 86644 files and directories currently installed.)
Preparing to unpack linux-headers-6.13.5-061305_6.13.5-061305.202502271338_all.deb ...
Unpacking linux-headers-6.13.5-061305 (6.13.5-061305.202502271338) ...
Selecting previously unselected package linux-headers-6.13.5-061305-generic.
Preparing to unpack linux-headers-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb ...
Unpacking linux-headers-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
Selecting previously unselected package linux-image-unsigned-6.13.5-061305-generic.
Preparing to unpack linux-image-unsigned-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb ...
Unpacking linux-image-unsigned-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
Selecting previously unselected package linux-modules-6.13.5-061305-generic.
Preparing to unpack linux-modules-6.13.5-061305-generic_6.13.5-061305.202502271338_amd64.deb ...
Unpacking linux-modules-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
Setting up linux-headers-6.13.5-061305 (6.13.5-061305.202502271338) ...
Setting up linux-headers-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
Setting up linux-modules-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
Setting up linux-image-unsigned-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
I: /boot/vmlinuz is now a symlink to vmlinuz-6.13.5-061305-generic
I: /boot/initrd.img is now a symlink to initrd.img-6.13.5-061305-generic
Processing triggers for linux-image-unsigned-6.13.5-061305-generic (6.13.5-061305.202502271338) ...
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-6.13.5-061305-generic
/etc/kernel/postinst.d/zz-update-grub:
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.13.5-061305-generic
Found initrd image: /boot/initrd.img-6.13.5-061305-generic
Found linux image: /boot/vmlinuz-6.8.0-54-generic
Found initrd image: /boot/initrd.img-6.8.0-54-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done

dsavostyanov@ubuntu-otus:~/kernel$  ls -al /boot
total 193472
drwxr-xr-x  4 root root     4096 Mar  2 13:56 .
drwxr-xr-x 23 root root     4096 Mar  2 12:13 ..
-rw-r--r--  1 root root   310720 Feb 27 13:38 config-6.13.5-061305-generic
-rw-r--r--  1 root root   287562 Feb  7 21:09 config-6.8.0-54-generic
drwxr-xr-x  5 root root     4096 Mar  2 13:56 grub
lrwxrwxrwx  1 root root       32 Mar  2 13:55 initrd.img -> initrd.img-6.13.5-061305-generic
-rw-r--r--  1 root root 78742093 Mar  2 13:56 initrd.img-6.13.5-061305-generic
-rw-r--r--  1 root root 68750099 Mar  2 12:21 initrd.img-6.8.0-54-generic
lrwxrwxrwx  1 root root       27 Mar  2 12:20 initrd.img.old -> initrd.img-6.8.0-54-generic
drwx------  2 root root    16384 Mar  2 12:14 lost+found
-rw-------  1 root root 10067508 Feb 27 13:38 System.map-6.13.5-061305-generic
-rw-------  1 root root  9080742 Feb  7 21:09 System.map-6.8.0-54-generic
lrwxrwxrwx  1 root root       29 Mar  2 13:55 vmlinuz -> vmlinuz-6.13.5-061305-generic
-rw-------  1 root root 15847936 Feb 27 13:38 vmlinuz-6.13.5-061305-generic
-rw-------  1 root root 14985608 Feb  7 22:01 vmlinuz-6.8.0-54-generic
lrwxrwxrwx  1 root root       24 Mar  2 12:20 vmlinuz.old -> vmlinuz-6.8.0-54-generic

dsavostyanov@ubuntu-otus:~/kernel$ sudo update-grub
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.13.5-061305-generic
Found initrd image: /boot/initrd.img-6.13.5-061305-generic
Found linux image: /boot/vmlinuz-6.8.0-54-generic
Found initrd image: /boot/initrd.img-6.8.0-54-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done
dsavostyanov@ubuntu-otus:~/kernel$ sudo grub-set-default 0
dsavostyanov@ubuntu-otus:~/kernel$ sudo reboot now

dsavostyanov@ubuntu-otus:~$ uname -r
6.13.5-061305-generic
```
