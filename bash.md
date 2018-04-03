
`~/.bashrc`
```sh
...
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
alias gcm='git commit -m'
alias gcam='git commit -am'
alias gco='git checkout'
alias ga='git add'

export EDITOR=nano
```
