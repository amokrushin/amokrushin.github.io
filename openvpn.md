iptables

```bash
sudo apt install iptables-persistent netfilter-persistent
sudo iptables -A FORWARD -o tun0 -j ACCEPT
sudo iptables -A FORWARD -i tun0 -j ACCEPT
sudo iptables -t nat -A POSTROUTING -s 192.168.255.0/24 -o enp9s0 -j MASQUERADE
sudo service netfilter-persistent save
```
