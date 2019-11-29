```
# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root   50G   47G  3.9G  93% /
devtmpfs                  12G     0   12G   0% /dev
tmpfs                     12G     0   12G   0% /dev/shm
tmpfs                     12G  804M   11G   7% /run
tmpfs                     12G     0   12G   0% /sys/fs/cgroup
/dev/sda1               1014M  189M  826M  19% /boot
/dev/mapper/centos-home  869G   68M  869G   1% /home
```

Смержить home в root

https://gist.github.com/troyfontaine/87091bd6a5c68f45dd62ced3d12bc377

```
mkdir home-backup
cp -a /home /home-backup/

```

```
# yum install lsof -y
lsof /home
# yum install psmisc
fuser -mv /home
```

```
umount -fl /home
lvremove /dev/centos/home
lvextend -l +100%FREE /dev/centos/root
xfs_growfs /dev/centos/root
```

```
cp -a /home-backup/home /
nano /etc/fstab
dracut --regenerate-all --force
```

```
# df -h
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/centos-root  919G   47G  873G   6% /
```
