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

* https://wiki.archlinux.org/index.php/Parted

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

Create LVM partition

```
$ parted /dev/sdb mkpart
Partition name?  []?
File system type?  [ext2]? ext4
Start? 538MB
End? 100%
Information: You may need to update /etc/fstab.

$ parted /dev/sdb set 2 lvm on

$ parted /dev/sdb print
Model: ATA ADATA SU650 (scsi)
Disk /dev/sdb: 240GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End    Size   File system  Name                  Flags
 1      1049kB  538MB  537MB  fat32        EFI System Partition  boot, esp
 2      538MB   240GB  240GB                                     lvm

```

https://help.ubuntu.ru/wiki/lvm

Инициализируйте физический диск c помощью pvcreate
```
$ pvcreate /dev/sdb2
  Physical volume "/dev/sdb2" successfully created.
```

Затем командой vgextend добавьте физический диск к существующей группе томов
```
$ vgextend ubuntu-vg /dev/sdb2
  Volume group "ubuntu-vg" successfully extended
```

```
$ vgdisplay
  --- Volume group ---
  VG Name               ubuntu-vg
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               688.32 GiB
  PE Size               4.00 MiB
  Total PE              176211
  Alloc PE / Size       119106 / <465.26 GiB
  Free  PE / Size       57105 / <223.07 GiB
  VG UUID               uEheAt-WlUH-G26M-7Wz2-pMyt-jcpe-c1HxWD
```

```
$ lvresize --resizefs --size 100G /dev/ubuntu-vg/root
fsck from util-linux 2.31.1
/dev/mapper/ubuntu--vg-root: 2158832/30433280 files (0.6% non-contiguous), 19593699/121713664 blocks
find: '/proc/4247': No such file or directory
resize2fs 1.44.1 (24-Mar-2018)
Resizing the filesystem on /dev/mapper/ubuntu--vg-root to 26214400 (4k) blocks.
The filesystem on /dev/mapper/ubuntu--vg-root is now 26214400 (4k) blocks long.

  Size of logical volume ubuntu-vg/root changed from 464.30 GiB (118861 extents) to 100.00 GiB (25600 ex$
  Logical volume ubuntu-vg/root successfully resized.
```

```
$ pvmove -i5 /dev/sda2 /dev/sdb2
  /dev/sda2: Moved: 0.05%
  /dev/sda2: Moved: 0.63%
  /dev/sda2: Moved: 1.21%
  /dev/sda2: Moved: 1.80%
  /dev/sda2: Moved: 2.39%
  /dev/sda2: Moved: 2.98%
  /dev/sda2: Moved: 3.55%
  /dev/sda2: Moved: 4.14%
  /dev/sda2: Moved: 4.72%
...
```

```
$ pvdisplay -m
  --- Physical volume ---
  PV Name               /dev/sda2
  VG Name               ubuntu-vg
  PV Size               <465.26 GiB / not usable 2.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              119106
  Free PE               119106
  Allocated PE          0
  PV UUID               2RfXy4-9746-adgG-MCXW-qZxy-2yDc-MGXZ2V

  --- Physical Segments ---
  Physical extent 0 to 119105:
    FREE

  --- Physical volume ---
  PV Name               /dev/sdb2
  VG Name               ubuntu-vg
  PV Size               <223.07 GiB / not usable 3.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              57105
  Free PE               31260
  Allocated PE          25845
  PV UUID               rJRlZU-5wg1-XEcv-DRwz-YrlT-WY7l-Pp7Dct

  --- Physical Segments ---
  Physical extent 0 to 25599:
    Logical volume	/dev/ubuntu-vg/root
    Logical extents     0 to 25599
  Physical extent 25600 to 25844:
    Logical volume	/dev/ubuntu-vg/swap_1
    Logical extents     0 to 244
  Physical extent 25845 to 57104:
    FREE
    
```

```
$ vgreduce ubuntu-vg /dev/sda2
  Removed "/dev/sda2" from volume group "ubuntu-vg"
```

```
$ pvremove /dev/sda2
  Labels on physical volume "/dev/sda2" successfully wiped.
```

```
$ pvdisplay
  --- Physical volume ---
  PV Name               /dev/sdb2
  VG Name               ubuntu-vg
  PV Size               <223.07 GiB / not usable 3.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              57105
  Free PE               31260
  Allocated PE          25845
  PV UUID               rJRlZU-5wg1-XEcv-DRwz-YrlT-WY7l-Pp7Dct
```

```
$ mkdir /mnt/prev-efi /mnt/next-efi
$ mount /dev/sda1 /mnt/prev-efi
$ mount /dev/sdb1 /mnt/next-efi
$ cp -a /mnt/prev-efi/* /mnt/next-efi
```

```
$ mount /dev/ubuntu-vg/root /mnt/next-root
$ lsblk -o name,uuid
$ nano /mnt/next-root/etc/fstab
```

```
$ umount /mnt/next-root
$ umount /mnt/next-efi
$ umount /mnt/prev-efi
```

Shut down, unmount hdd, reboot

```
$ lvextend --resizefs -l +100%FREE /dev/ubuntu-vg/root
  Size of logical volume ubuntu-vg/root changed from 100.00 GiB (25600 extents) to <222.11 GiB (56860 ex$
  Logical volume ubuntu-vg/root successfully resized.
resize2fs 1.44.1 (24-Mar-2018)
Filesystem at /dev/mapper/ubuntu--vg-root is mounted on /; on-line resizing required
old_desc_blocks = 13, new_desc_blocks = 28
The filesystem on /dev/mapper/ubuntu--vg-root is now 58224640 (4k) blocks long.
```
