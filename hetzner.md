# Hetzner Dedicated

## EX41-SSD

Hardware data:

   CPU1: Intel(R) Core(TM) i7-7700 CPU @ 3.60GHz (Cores 8)
   Memory:  31958 MB
   Disk /dev/sda: 512 GB (=> 476 GiB)
   Disk /dev/sdb: 512 GB (=> 476 GiB)
   Total capacity 953 GiB with 2 Disks

Network data:
   eth0  LINK: yes
         MAC:  90:xx:xx:xx:xx:xx
         IP:   95.xx.xx.xx
         IPv6: 2a01:xxx:xx:xxxx::x/64
         Intel(R) PRO/1000 Network Driver

## installimage

- [Installation of Hetzner Servers](https://www.next-level-help.org/nlidocu/html/default/Installation_of_Hetzner_Servers.html)
- [Setup LVM on Hetzner and use all drives](https://www.alexdumitru.com/setup-lvm-on-hetzner-and-use-all-drives/)

```
DRIVE1 /dev/sda
DRIVE2 /dev/sdb
SWRAID 1
SWRAIDLEVEL 1

PART  /boot  ext3  1024M
PART  lvm    vg0   all

LV  vg0  root  /     ext4  all
LV  vg0  swap  swap  swap  16G
```
