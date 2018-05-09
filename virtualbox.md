## Headless on Ubuntu Server 16.04

<https://www.virtualbox.org/wiki/Linux_Downloads>

<https://websiteforstudents.com/virtualbox-5-2-on-ubuntu-16-04-lts-server-headless/>

<https://www.ostechnix.com/install-oracle-virtualbox-ubuntu-16-04-headless-server/>

<https://www.techrepublic.com/article/how-to-import-and-export-virtualbox-appliances-from-the-command-line/>

<https://github.com/Jencryzthers/VboxInsideDocker/blob/master/Dockerfile>

<https://www.virtualbox.org/manual/ch07.html>

<https://github.com/xdissent/ievms>

<https://github.com/xdissent/iectrl>

<http://xmodulo.com/how-to-create-and-start-virtualbox-vm-without-gui.html>

<https://stackoverflow.com/questions/25741904/is-it-possible-to-run-virtualbox-inside-a-docker-container>

```sh
sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get autoremove
sudo apt-get -y install gcc make linux-headers-$(uname -r) dkms

wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list'

sudo apt-get update
sudo apt-get install virtualbox-5.2
VBoxManage -v


# Install VirtualBox Extension Pack

# curl -O http://download.virtualbox.org/virtualbox/5.2.4/Oracle_VM_VirtualBox_Extension_Pack-5.2.4-119785.vbox-extpack
# sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.2.4-119785.vbox-extpack

VBOX_VERSION=`dpkg -s virtualbox-5.2 | grep '^Version: ' | sed -e 's/Version: \([0-9\.]*\)\-.*/\1/'` ; \
VBOX_LICENSE=56be48f923303c8cababb0bb4c478284b688ed23f16d775d729b89a2e8e5f9eb ; \
wget http://download.virtualbox.org/virtualbox/${VBOX_VERSION}/Oracle_VM_VirtualBox_Extension_Pack-${VBOX_VERSION}.vbox-extpack ; \
VBoxManage extpack install --accept-license=${VBOX_LICENSE} Oracle_VM_VirtualBox_Extension_Pack-${VBOX_VERSION}.vbox-extpack ; \
rm Oracle_VM_VirtualBox_Extension_Pack-${VBOX_VERSION}.vbox-extpack

VBoxManage list extpacks

```

```
curl -s https://raw.githubusercontent.com/xdissent/ievms/master/ievms.sh | env IEVMS_VERSIONS="10" bash
```

<https://github.com/magnetikonline/linux-microsoft-ie-virtual-machines>

```
vboxmanage list vms
vboxmanage import ubuntu_server_new.ova
vboxmanage export UBUNTUSERVER164 -o ubuntu_server_new.ova
```

<https://medium.com/@dbillinghamuk/setup-guide-for-running-selenium-functional-tests-in-webdriver-io-against-ie9-on-vm-1928e83f37ff>
