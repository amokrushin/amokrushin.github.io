# SSH

## Readme

- [SSH (SECURE SHELL)](https://www.ssh.com/ssh/)
- [How To Configure Custom Connection Options for your SSH Client](https://www.digitalocean.com/community/tutorials/how-to-configure-custom-connection-options-for-your-ssh-client)
- [SSH Essentials: Working with SSH Servers, Clients, and Keys](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
- [Using SSH-Agent the right way in Windows 10 WSL2](https://esc.sh/blog/ssh-agent-windows10-wsl2/)

## Configuration

```
ssh-keygen -t ed25519 -C "anton@mokr.org"
```


`~/.ssh/config`

```bash
Host example.com
  User am
  HostName example.com
  Port 22
  IdentityFile ~/.ssh/id_ed25519
  ServerAliveInterval 60
  ServerAliveCountMax 10
```

```
sudo apt install keychain
```

`~/.bashrc`
```
# Loading the SSH key
/usr/bin/keychain -q --nogui $HOME/.ssh/id_ed25519
source $HOME/.keychain/$HOSTNAME-sh
```

```
ssh-copy-id -i ~/.ssh/id_ed25519 am@host
```

## scp

```
# copy from remote machine
scp example.com:/home/am/foo.txt /home/am/

# copy to remote machine
scp /home/am/foo.txt example.com:/home/am/
```

```
 -p      Preserves modification times, access times, and modes from the original file.
 -q      Quiet mode: disables the progress meter as well as warning and diagnostic messages from ssh(1).
 -r      Recursively copy entire directories.  Note that scp follows symbolic links encountered in the tree
         traversal.
```

## scp-tools

```
sudo apt-get install pv
```

`~/.bashrc`
```
export PATH=~/bin:$PATH
```

`~/bin/scp-from`

```bash
#!/usr/bin/env bash

set -e

arg_host=$1
from_dirname=$(dirname $2)
from_basename=$(basename  $2)
to_dirname=$3

ssh ${arg_host} exit
ssh ${arg_host} "tar -C ${from_dirname} -cf - ${from_basename}" | pv | tar -xf - -C ${to_dirname}
```
## reverse autossh

`/etc/systemd/system/examplecomtun.service`

```
[Unit]
Description=Example.com reverse SSH
Requires=systemd-networkd-wait-online.service
After=systemd-networkd-wait-online.service

[Service]
ExecStart=/usr/bin/autossh \
        -M 2223 \
        -N \
        -q \
        -o ServerAliveInterval=10 \
        -o ServerAliveCountMax=3 \
        -o ExitOnForwardFailure=yes \
        -p 9358 \
        -i /root/.ssh/key \
        -R 2222:localhost:22 \
        user@example.com

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
systemctl enable examplecomtun.service
systemctl start examplecomtun.service
```
