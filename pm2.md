## PM2

### Shared development group

`~/.bashrc`

```
...
export PM2_HOME=/opt/pm2-development
```

```
sudo mkdir /opt/pm2-development
sudo groupadd pm2-development
sudo chgrp -R pm2-development /opt/pm2-development
sudo usermod -aG pm2-development am
sudo chmod g+w /opt/pm2-development
```
