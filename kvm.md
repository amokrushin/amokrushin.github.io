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
$ sudo apt install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
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
