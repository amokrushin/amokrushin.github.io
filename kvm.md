# KVM

## Installation

[Ubuntu Documentation: KVM/Installation](https://help.ubuntu.com/community/KVM/Installation)

Pre-installation check
```bash
$ egrep -c '(vmx|svm)' /proc/cpuinfo
8

$ sudo apt install cpu-checker
$ sudo kvm-ok
INFO: /dev/kvm exists
KVM acceleration can be used
```

Install
```bash
$ sudo apt update
$ sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
$ sudo adduser `id -un` libvirt
```

Verify Installation
```bash
$ virsh list --all
 Id    Name                           State
----------------------------------------------------

$ sudo ls -la /var/run/libvirt/libvirt-sock
srwxrwx--- 1 root libvirtd 0 Oct  6 00:18 /var/run/libvirt/libvirt-sock

$ ls -l /dev/kvm
crw-rw---- 1 root kvm 10, 232 Oct  6 02:02 /dev/kvm
```

## Networking

[Ubuntu Documentation: KVM/Networking](https://help.ubuntu.com/community/KVM/Networking)

Usermode Networking

```sh
$ virsh net-list --all
 Name                 State      Autostart     Persistent
----------------------------------------------------------
 default              active     yes           yes

# If missing
$ cp /etc/libvirt/libvirt.conf ~/.config/libvirt/libvirt.conf
$ nano ~/.config/libvirt/libvirt.conf
# Uncomment line
uri_default = "qemu:///system"
 
# If inactive
$ virsh net-start default
Network default started

$ brctl show
bridge name     bridge id               STP enabled     interfaces
virbr0          8000.52540030b6e1       yes             virbr0-nic

$ sudo nano /etc/sysctl.conf
# Uncomment the next line to enable packet forwarding for IPv4
net.ipv4.ip_forward=1

sudo sysctl -p
```

## CreateGuests

[KVM Guest Support Status](http://www.linux-kvm.org/page/Guest_Support_Status)

