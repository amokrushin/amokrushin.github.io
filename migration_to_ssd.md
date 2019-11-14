* https://www.ibm.com/developerworks/ru/library/l-lvm2/index.html


Исходные данные

```
$ parted -l
Model: ATA WDC WD5000AAKX-0 (scsi)
Disk /dev/sda: 500GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End    Size   File system  Name                  Flags
 1      1049kB  538MB  537MB  fat32        EFI System Partition  boot, esp
 2      538MB   500GB  500GB                                     lvm


Error: /dev/sdb: unrecognised disk label
Model: ATA ADATA SU650 (scsi)
Disk /dev/sdb: 240GB
Sector size (logical/physical): 512B/512B
Partition Table: unknown
Disk Flags:


Model: Linux device-mapper (linear) (dm)
Disk /dev/mapper/ubuntu--vg-swap_1: 1028MB
Sector size (logical/physical): 512B/512B
Partition Table: loop
Disk Flags:

Number  Start  End     Size    File system     Flags
 1      0.00B  1028MB  1028MB  linux-swap(v1)


Model: Linux device-mapper (linear) (dm)
Disk /dev/mapper/ubuntu--vg-root: 499GB
Sector size (logical/physical): 512B/512B
Partition Table: loop
Disk Flags:

Number  Start  End    Size   File system  Flags
 1      0.00B  499GB  499GB  ext4

```

```
$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/ubuntu--vg-root /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=F0D5-218F  /boot/efi       vfat    umask=0077      0       1
/dev/mapper/ubuntu--vg-swap_1 none            swap    sw              0       0

```


Create new partition table

```
$ parted /dev/sdb mklabel gpt
Information: You may need to update /etc/fstab.

$ parted /dev/sdb print
Model: ATA ADATA SU650 (scsi)
Disk /dev/sdb: 240GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start  End  Size  File system  Name  Flags

```


Create an ESP (EFI System Partition)

* https://access.redhat.com/sites/default/files/attachments/parted_0.pdf
* https://wiki.archlinux.org/index.php/EFI_system_partition_(%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9)

```
$ parted /dev/sdb mkpart '"EFI System Partition"' fat32 1049kB 538MB
$ parted /dev/sdb set 1 esp on
$ mkfs.fat -F32 /dev/sdb1

$ parted /dev/sdb print
Model: ATA ADATA SU650 (scsi)
Disk /dev/sdb: 240GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End    Size   File system  Name                  Flags
 1      1049kB  538MB  537MB  fat32        EFI System Partition  boot, esp

```
