## Headless on Ubuntu Server 16.04

<https://www.virtualbox.org/wiki/Linux_Downloads>

<https://websiteforstudents.com/virtualbox-5-2-on-ubuntu-16-04-lts-server-headless/>

```sh
sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get autoremove
sudo apt-get -y install gcc make linux-headers-$(uname -r) dkms

wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list'

sudo apt-get update
sudo apt-get install virtualbox-5.2
VBoxManage -v

curl -O http://download.virtualbox.org/virtualbox/5.2.4/Oracle_VM_VirtualBox_Extension_Pack-5.2.4-119785.vbox-extpack
sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.2.4-119785.vbox-extpack
VBoxManage list extpacks
```
