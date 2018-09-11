# Ubuntu 16.04, 18.04

## FAQ

- [How To Add Swap Space on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-16-04)
  > If fallocate fails or it not available, you can use dd:
  > ```
  > sudo dd if=/dev/zero of=/mnt/1GiB.swap bs=1024 count=1048576
  > ```
  
- [Command-line tool for controlling NetworkManager](http://manpages.ubuntu.com/manpages/bionic/man1/nmcli.1.html)

  `nmcli connection` - List connections<br>
  `nmcli connection up/down id name` - Up/down connection by id
  
  **Troobleshooting**

  > `Error: Connection activation failed: Not authorized to control networking.`<br>
  > Then check permissions `nmcli general permissions` and [fix](https://askubuntu.com/a/752168) PolicyKit rule for the NetworkManager.

  > How to disable Alt-Arrow switching of Virtual Consoles?<br>
  > `sudo kbd_mode -s`


## Initial setup

```
apt-get update \
&& apt-get dist-upgrade -y \
&& apt-get install -y nano curl
```

### User
```bash
passwd \
&& adduser am \
&& usermod -aG sudo am \
&& su am
# > enter root account new password
# > enter am account new password
```

### sshd

```bash
mkdir -p ~/.ssh \
&& chmod 0700 ~/.ssh \
&& touch ~/.ssh/authorized_keys \
&& chmod 0644 ~/.ssh/authorized_keys \
&& nano ~/.ssh/authorized_keys
# > paste am account public key
```

```bash
sudo nano /etc/ssh/sshd_config \
  && sudo service ssh reload
# set Port <custom port number>
# set PermitRootLogin no
# set PasswordAuthentication no
# set UsePAM no
```

### .bashrc
```
export NCURSES_NO_UTF8_ACS=1
export LC_ALL=en_US.utf8

alias yi='yarn add'
alias yr='yarn remove'
alias yu='yarn upgrade'
alias yui='yarn upgrade-interactive'

alias dc='docker-compose'
alias dcl='dc logs -f --tail 0'
alias dcs='docker stats $(docker-compose ps -q || echo "#")'
alias dcps='docker ps $((docker-compose ps -q  || echo "#") | while read line; do echo "--filter id=$line"; done)'

alias gs='git status'
alias gd='git diff'
alias gl='git log'
alias gcm='git commit -m'
alias gcam='git commit -am'
alias gco='git checkout'
alias ga='git add'
alias gcl='git clone'
alias gpl='git pull'
alias gps='git push'
alias gflc='git diff-tree --no-commit-id --name-only -r $(git rev-parse HEAD)'
alias gdlc='git diff HEAD^ HEAD'
alias git-reset='git reset --hard HEAD && git clean -df && git pull'

alias chome='sudo chown -R $(whoami):$(whoami)'

if [ -z "${SSH_AUTH_SOCK}" ] ; then
    eval `ssh-agent -s`
fi

export EDITOR=nano
```

### Node.js

```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash - \
&& sudo apt-get install -y nodejs build-essential
```
> <https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions>

`.npmrc`
```
init-author-name=Anton Mokrushin
init-author-email=anton@mokr.org
init-version=0.1.0
init-license=MIT
```

### Yarn

```
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add - \
&& echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list \
&& sudo apt-get update \
&& sudo apt-get install yarn
```
> <https://yarnpkg.com/en/docs/install>

### Git

```
sudo apt-get install git -y
git config --global user.name "Anton Mokrushin" \
&& git config --global user.email "anton@mokr.org" \
&& git config --global credential.helper 'store --file ~/.git-credentials'
```

### Docker

<https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/>

<https://docs.docker.com/compose/install/>

```
sudo usermod -aG docker am
```
