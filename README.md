# Загрузка системы_Работа с загрузчиком
```
Включить отображение меню Grub

dsavostyanov@otus:~$ sudo nano /etc/default/grub
[sudo] password for dsavostyanov:

dsavostyanov@otus:~$ update-grub
grub-mkconfig: You must run this as root
dsavostyanov@otus:~$ sudo update-grub
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-6.8.0-56-generic
Found initrd image: /boot/initrd.img-6.8.0-56-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done

Попасть в систему без пароля несколькими способами

Установить систему с LVM, после чего переименовать VG

root@otus:~# vgs
  VG        #PV #LV #SN Attr   VSize  VFree
  ubuntu-vg   1   1   0 wz--n- 18.22g 8.22g
  
root@otus:~# vgrename ubuntu-vg ubuntu-otus
  Volume group "ubuntu-vg" successfully renamed to "ubuntu-otus"
  
dsavostyanov@otus:~$ sudo vgs
[sudo] password for dsavostyanov:
  VG          #PV #LV #SN Attr   VSize  VFree
  ubuntu-otus   1   1   0 wz--n- 18.22g 8.22g
```
