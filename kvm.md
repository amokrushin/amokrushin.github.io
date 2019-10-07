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

- **[OpenStack Virtual Machine Image Guide](https://docs.openstack.org/image-guide/index.html)**
  - [Modify images](https://docs.openstack.org/image-guide/modify-images.html)
  - [Convert images](https://docs.openstack.org/image-guide/convert-images.html)
- [Using Cloud Images in KVM](https://www.theurbanpenguin.com/using-cloud-images-in-kvm/)
- [cloud-init docs](https://cloudinit.readthedocs.io/en/latest/index.html)

Cloud Images
* Ubuntu: https://cloud-images.ubuntu.com/
* CentOS: https://cloud.centos.org/centos/

Install Tools
```
sudo apt update
sudo apt install libguestfs-tools
```

Download cloud image
```
mkdir -p ~/virt/images
cd ~/virt/images
wget https://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud-1708.qcow2.xz
xz -v -d CentOS-7-x86_64-GenericCloud-1708.qcow2.xz

$ qemu-img info CentOS-7-x86_64-GenericCloud-1708.qcow2
image: CentOS-7-x86_64-GenericCloud-1708.qcow2
file format: qcow2
virtual size: 8.0G (8589934592 bytes)
disk size: 807M
cluster_size: 65536
Format specific information:
    compat: 0.10
    refcount bits: 16
```

To be continued
```
/var/lib/libvirt/images
```

* https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/virtualization_administration_guide/create-lvm-storage-pool-virsh
