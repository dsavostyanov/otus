# Занятие 1. Vagrant-стенд для обновления ядра и создания образа системы
```
C:\Temp\Otus\vagrant>git clone https://github.com/Nickmob/vagrant_kernel_update
Cloning into 'vagrant_kernel_update'...
remote: Enumerating objects: 31, done.
remote: Counting objects: 100% (31/31), done.
remote: Compressing objects: 100% (29/29), done.
remote: Total 31 (delta 10), reused 5 (delta 0), pack-reused 0 (from 0)
Receiving objects: 100% (31/31), 7.80 KiB | 998.00 KiB/s, done.
Resolving deltas: 100% (10/10), done.

C:\Temp\Otus\vagrant\vagrant_kernel_update>vagrant up
Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Box 'generic/centos8s' could not be found. Attempting to find and install...
    kernel-update: Box Provider: virtualbox
    kernel-update: Box Version: 4.3.4
==> kernel-update: Loading metadata for box 'generic/centos8s'
    kernel-update: URL: https://vagrantcloud.com/api/v2/vagrant/generic/centos8s
==> kernel-update: Adding box 'generic/centos8s' (v4.3.4) for provider: virtualbox (amd64)
    kernel-update: Downloading: https://vagrantcloud.com/generic/boxes/centos8s/versions/4.3.4/providers/virtualbox/amd64/vagrant.box
    kernel-update:
    kernel-update: Calculating and comparing box checksum...
==> kernel-update: Successfully added box 'generic/centos8s' (v4.3.4) for 'virtualbox (amd64)'!
==> kernel-update: Importing base box 'generic/centos8s'...
==> kernel-update: Matching MAC address for NAT networking...
==> kernel-update: Checking if box 'generic/centos8s' version '4.3.4' is up to date...
==> kernel-update: Setting the name of the VM: vagrant_kernel_update_kernel-update_1749812191179_48427
==> kernel-update: Clearing any previously set network interfaces...
==> kernel-update: Preparing network interfaces based on configuration...
    kernel-update: Adapter 1: nat
==> kernel-update: Forwarding ports...
    kernel-update: 22 (guest) => 2222 (host) (adapter 1)
==> kernel-update: Running 'pre-boot' VM customizations...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
    kernel-update: SSH address: 127.0.0.1:2222
    kernel-update: SSH username: vagrant
    kernel-update: SSH auth method: private key
    kernel-update:
    kernel-update: Vagrant insecure key detected. Vagrant will automatically replace
    kernel-update: this with a newly generated keypair for better security.
    kernel-update:
    kernel-update: Inserting generated public key within guest...
    kernel-update: Removing insecure key from the guest if it's present...
    kernel-update: Key inserted! Disconnecting and reconnecting using new SSH key...
==> kernel-update: Machine booted and ready!
==> kernel-update: Checking for guest additions in VM...
    kernel-update: The guest additions on this VM do not match the installed version of
    kernel-update: VirtualBox! In most cases this is fine, but in rare cases it can
    kernel-update: prevent things such as shared folders from working properly. If you see
    kernel-update: shared folder errors, please make sure the guest additions within the
    kernel-update: virtual machine match the version of VirtualBox you have installed on
    kernel-update: your host and reload your VM.
    kernel-update:
    kernel-update: Guest Additions Version: 6.1.30
    kernel-update: VirtualBox Version: 7.1
==> kernel-update: Setting hostname...

[vagrant@kernel-update ~]$ uname -r
4.18.0-516.el8.x86_64

[vagrant@kernel-update ~]$ sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
CentOS Stream 8 - AppStream                                                                                                                                                                                  0.0  B/s |   0  B     00:02
Errors during downloading metadata for repository 'appstream':
  - Curl error (6): Couldn't resolve host name for http://mirrorlist.centos.org/?release=8-stream&arch=x86_64&repo=AppStream&infra=stock [Could not resolve host: mirrorlist.centos.org]
Error: Failed to download metadata for repo 'appstream': Cannot prepare internal mirrorlist: Curl error (6): Couldn't resolve host name for http://mirrorlist.centos.org/?release=8-stream&arch=x86_64&repo=AppStream&infra=stock [Could not resolve host: mirrorlist.centos.org]

[vagrant@kernel-update ~]$ sudo -i

[vagrant@kernel-update ~]$ vi /etc/yum.repos.d/CentOS-AppStream.repo
#mirrorlist=http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=extras-extras-common
baseurl=http://vault.centos.org/8.5.2111/AppStream/$basearch/os/

[vagrant@kernel-update ~]$ vi/etc/yum.repos.d/CentOS-Base.rep
#mirrorlist=http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=extras-extras-common
baseurl=http://vault.centos.org/8.5.2111/AppStream/$basearch/os/

[vagrant@kernel-update ~]$ vi /etc/yum.repos.d/CentOS-Stream-Extras.repo
#mirrorlist=http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=extras-extras-common
baseurl=http://vault.centos.org/8.5.2111/AppStream/$basearch/os/

[vagrant@kernel-update ~]$ vi /etc/yum.repos.d/CentOS-Stream-Extras-common.repo
#mirrorlist=http://mirrorlist.centos.org/?release=$stream&arch=$basearch&repo=extras-extras-common
baseurl=http://vault.centos.org/8.5.2111/AppStream/$basearch/os/

[root@kernel-update ~]# sudo yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
Last metadata expiration check: 0:03:12 ago on Fri 13 Jun 2025 07:43:33 PM UTC.
elrepo-release-8.el8.elrepo.noarch.rpm                                                                                                                                                                       4.8 kB/s |  19 kB     00:03
Dependencies resolved.
=============================================================================================================================================================================================================================================
 Package                                                    Architecture                                       Version                                                        Repository                                                Size
=============================================================================================================================================================================================================================================
Installing:
 elrepo-release                                             noarch                                             8.4-2.el8.elrepo                                               @commandline                                              19 k

Transaction Summary
=============================================================================================================================================================================================================================================
Install  1 Package

Total size: 19 k
Installed size: 8.3 k
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                     1/1
  Installing       : elrepo-release-8.4-2.el8.elrepo.noarch                                                                                                                                                                              1/1
  Verifying        : elrepo-release-8.4-2.el8.elrepo.noarch                                                                                                                                                                              1/1

Installed:
  elrepo-release-8.4-2.el8.elrepo.noarch

Complete!

[root@kernel-update ~]# sudo yum --enablerepo elrepo-kernel install kernel-ml -y
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                                                                                348 kB/s | 2.8 MB     00:08
Dependencies resolved.
=============================================================================================================================================================================================================================================
 Package                                                     Architecture                                     Version                                                          Repository                                               Size
=============================================================================================================================================================================================================================================
Installing:
 kernel-ml                                                   x86_64                                           6.15.2-1.el8.elrepo                                              elrepo-kernel                                           154 k
Installing dependencies:
 kernel-ml-core                                              x86_64                                           6.15.2-1.el8.elrepo                                              elrepo-kernel                                            67 M
 kernel-ml-modules                                           x86_64                                           6.15.2-1.el8.elrepo                                              elrepo-kernel                                            62 M

Transaction Summary
=============================================================================================================================================================================================================================================
Install  3 Packages

Total download size: 129 M
Installed size: 174 M
Downloading Packages:
(1/3): kernel-ml-6.15.2-1.el8.elrepo.x86_64.rpm                                                                                                                                                               95 kB/s | 154 kB     00:01
(2/3): kernel-ml-modules-6.15.2-1.el8.elrepo.x86_64.rpm                                                                                                                                                      309 kB/s |  62 MB     03:25
(3/3): kernel-ml-core-6.15.2-1.el8.elrepo.x86_64.rpm                                                                                                                                                         280 kB/s |  67 MB     04:04
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                                                                        539 kB/s | 129 MB     04:04
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                                                                                1.6 MB/s | 1.7 kB     00:00
Importing GPG key 0xBAADAE52:
 Userid     : "elrepo.org (RPM Signing Key for elrepo.org) <secure@elrepo.org>"
 Fingerprint: 96C0 104F 6315 4731 1E0B B1AE 309B C305 BAAD AE52
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
Key imported successfully
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                                                                                3.0 MB/s | 3.1 kB     00:00
Importing GPG key 0xEAA31D4A:
 Userid     : "elrepo.org (RPM Signing Key v2 for elrepo.org) <secure@elrepo.org>"
 Fingerprint: B8A7 5587 4DA2 40C9 DAC4 E715 5160 0989 EAA3 1D4A
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-v2-elrepo.org
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                                                                                     1/1
  Installing       : kernel-ml-core-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                           1/3
  Running scriptlet: kernel-ml-core-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                           1/3
  Installing       : kernel-ml-modules-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                        2/3
  Running scriptlet: kernel-ml-modules-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                        2/3
  Installing       : kernel-ml-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                                3/3
  Running scriptlet: kernel-ml-core-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                           3/3
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y

  Running scriptlet: kernel-ml-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                                3/3
  Verifying        : kernel-ml-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                                1/3
  Verifying        : kernel-ml-core-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                           2/3
  Verifying        : kernel-ml-modules-6.15.2-1.el8.elrepo.x86_64                                                                                                                                                                        3/3

Installed:
  kernel-ml-6.15.2-1.el8.elrepo.x86_64                                      kernel-ml-core-6.15.2-1.el8.elrepo.x86_64                                      kernel-ml-modules-6.15.2-1.el8.elrepo.x86_64

Complete!

[root@kernel-update ~]# ls -al /boot
total 359356
dr-xr-xr-x.  5 root root      4096 Jun 13 19:55 .
dr-xr-xr-x. 17 root root       224 Oct 17  2023 ..
-rw-r--r--.  1 root root    202117 Oct  2  2023 config-4.18.0-516.el8.x86_64
-rw-r--r--.  1 root root    259381 Jun 10 17:20 config-6.15.2-1.el8.elrepo.x86_64
drwxr-xr-x.  3 root root        17 Oct 17  2023 efi
drwx------.  4 root root        83 Jun 13 19:34 grub2
-rw-------.  1 root root  91146339 Oct 17  2023 initramfs-0-rescue-114f3f94a1f54f5eab6b51b8f9589400.img
-rw-------.  1 root root 122973423 Jun 13 19:55 initramfs-0-rescue-bda3212789204e88a7c099e47d33b5a8.img
-rw-------.  1 root root  29951372 Oct 17  2023 initramfs-4.18.0-516.el8.x86_64.img
-rw-------.  1 root root  27778560 Oct 17  2023 initramfs-4.18.0-516.el8.x86_64kdump.img
-rw-------.  1 root root  32953878 Jun 13 19:54 initramfs-6.15.2-1.el8.elrepo.x86_64.img
drwxr-xr-x.  3 root root        21 Oct 17  2023 loader
lrwxrwxrwx.  1 root root        45 Oct 17  2023 symvers-4.18.0-516.el8.x86_64.gz -> /lib/modules/4.18.0-516.el8.x86_64/symvers.gz
lrwxrwxrwx.  1 root root        50 Jun 13 19:53 symvers-6.15.2-1.el8.elrepo.x86_64.gz -> /lib/modules/6.15.2-1.el8.elrepo.x86_64/symvers.gz
-rw-------.  1 root root   4487452 Oct  2  2023 System.map-4.18.0-516.el8.x86_64
-rw-------.  1 root root   7674634 Jun 10 17:20 System.map-6.15.2-1.el8.elrepo.x86_64
-rwxr-xr-x.  1 root root  10913504 Oct 17  2023 vmlinuz-0-rescue-114f3f94a1f54f5eab6b51b8f9589400
-rwxr-xr-x.  1 root root  14344704 Jun 13 19:54 vmlinuz-0-rescue-bda3212789204e88a7c099e47d33b5a8
-rwxr-xr-x.  1 root root  10913504 Oct  2  2023 vmlinuz-4.18.0-516.el8.x86_64
-rw-r--r--.  1 root root       166 Oct  2  2023 .vmlinuz-4.18.0-516.el8.x86_64.hmac
-rwxr-xr-x.  1 root root  14344704 Jun 10 17:20 vmlinuz-6.15.2-1.el8.elrepo.x86_64

[root@kernel-update ~]# sudo grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
done

[root@kernel-update ~]# sudo grub2-set-default 0
[root@kernel-update ~]# reboot

[vagrant@kernel-update ~]$ uname -r
6.15.2-1.el8.elrepo.x86_64
```
