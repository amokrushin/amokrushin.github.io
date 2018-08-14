# systemctl

[How To Use Systemctl to Manage Systemd Services and Units](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units)

```
systemctl list-unit-files | grep enabled
systemctl list-unit-files | grep docker

# override or add directives to the unit definition
systemctl edit docker.service

# edit the full unit file instead of creating a snippet
sudo systemctl edit --full docker.service
```
