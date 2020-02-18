#### docker-compose.yml

```yml
version: '3.6'

services:
  openvpn:
    image: kylemanna/openvpn
    cap_add:
     - NET_ADMIN
    ports:
     - 1194:1194/udp
    volumes:
     - ./data/openvpn:/etc/openvpn
    restart: unless-stopped
```

#### ./data/openvpn/ccd/${client_name}

```
iroute 10.0.0.0 255.0.0.0
```

#### ./data/openvpn/openvpn.conf

```
...
client-to-client
client-config-dir ccd

route 10.0.0.0 255.0.0.0
push "route 10.0.0.0 255.0.0.0"
```

#### CLI

```bash
export SERVERNAME="example.com"
export CLIENTNAME="example-client"

dc run --rm openvpn ovpn_genconfig -u udp://${SERVERNAME}:1194
dc run --rm openvpn ovpn_initpki

dc run --rm openvpn easyrsa build-client-full ${CLIENTNAME}
dc run --rm openvpn ovpn_getclient $CLIENTNAME > ${CLIENTNAME}.ovpn

dc exec openvpn ovpn_listclients
dc exec openvpn ovpn_status
```

#### iptables

```bash
sudo apt install iptables-persistent netfilter-persistent
sudo iptables -A FORWARD -o tun0 -j ACCEPT
sudo iptables -A FORWARD -i tun0 -j ACCEPT
sudo iptables -t nat -A POSTROUTING -s 192.168.255.0/24 -o enp9s0 -j MASQUERADE
sudo service netfilter-persistent save
```
