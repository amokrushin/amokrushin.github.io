# Роскомпозор ебанулся вкрай!

> <s>Новость завтрашнего дня:</s> В результате попыток Роскомнадзора по блокированию Телеграм в России перестало работать всё, кроме Телеграм.<br/>
> *[#comment_10763018](https://habrahabr.ru/post/353822/#comment_10763018)*

[Отслеживание количества актуальных IP адресов из выгрузки Роскомнадзора](https://usher2.club/)

[t.me/rknshowtime - Shows current IP blocked by RKN](https://t.me/rknshowtime)




## IPsec VPN

### Server

[github.com/hwdsl2/docker-ipsec-vpn-server](https://github.com/hwdsl2/docker-ipsec-vpn-server)

### Windows 10 client

- [Connect automatically after log on](https://answers.microsoft.com/en-us/insider/forum/insider_wintp-insider_web-insiderplat_pc/can-i-set-my-vpn-conection-to-connect/c6a1e7f2-9cee-42ec-9f98-7fcf2b3a3a42)
- [Reconnect automatically](https://rushtips.com/windows-10-vpn-reconnect-automatically)

### Ubuntu 16.04

```
sudo add-apt-repository ppa:nm-l2tp/network-manager-l2tp \
&& sudo apt-get update \
&& sudo apt-get install network-manager-l2tp network-manager-l2tp-gnome
```
> *[How to add the L2TP VPN option to NetworkManager in Linux](https://www.techrepublic.com/article/how-to-add-the-l2tp-vpn-option-to-network-manager-in-linux/)*




## OpenVPN

### Server

```yml
version: '3.6'

services:
  openvpn:
    image: kylemanna/openvpn
    cap_add:
     - NET_ADMIN
    ports:
     - 443:1194/tcp
    volumes:
     - ./data/openvpn:/etc/openvpn
    restart: unless-stopped
```

```sh
export SERVERNAME="example.com"
export CLIENTNAME="your_client_name"

dc run --rm openvpn ovpn_genconfig -u tcp://${SERVERNAME}:443
dc run --rm openvpn ovpn_initpki

dc run --rm openvpn easyrsa build-client-full ${CLIENTNAME}
dc run --rm openvpn ovpn_getclient $CLIENTNAME > ${CLIENTNAME}.ovpn
```
> * [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn)
>   - [docs/docker-compose.md](https://github.com/kylemanna/docker-openvpn/blob/master/docs/docker-compose.md)
>   - [docs/tcp.md](https://github.com/kylemanna/docker-openvpn/blob/master/docs/tcp.md)

```
dc exec openvpn ovpn_listclients
dc exec openvpn ovpn_status
```

### Windows 10 client

[Downloads](https://openvpn.net/index.php/open-source/downloads.html)




## DNS

[dnscrypt.info](https://dnscrypt.info/)

[simplednscrypt.org](https://simplednscrypt.org/)

[Как спрятать DNS-запросы от любопытных глаз провайдера](https://habrahabr.ru/post/353878/)

[DnsCrypt on Ubuntu – Encrypted DNS Traffic](https://linuxhint.com/dnscrypt-ubuntu/)

[DNSCrypt-proxy on Ubuntu 16.04](https://sorenpoulsen.com/dnscrypt-proxy-on-ubuntu-1604)

[Howto: use a DNSCrypt resolver + local caching DNS proxy on Debian desktop](https://www.reddit.com/r/linux/comments/49eds2/howto_use_a_dnscrypt_resolver_local_caching_dns/)

## Test

```
curl ipconfig.io
curl ipinfo.io
```

- [Test OpenDNS](https://welcome.opendns.com/)
- [ipleak.net](https://ipleak.net/)

## Example

```
#
# /etc/dnsmasq.conf
#

 ignore resolv.conf
no-resolv

# Listen only on localhost
listen-address=127.0.0.1

local=/local/
server=/example/127.0.1.1

server=127.0.2.1
proxy-dnssec
```

## Docker registry proxy

### Server

`docker-compose.yml`
```
version: '3.6'

services:
  docker-io-proxy:
    build: https://github.com/amokrushin/docker-io-proxy.git
    ports:
      - 1080:1080
    volumes:
      - ./data/docker-io-proxy:/etc
    restart: unless-stopped
```

### Client

https://docs.docker.com/config/daemon/systemd/#httphttps-proxy

> The Docker daemon uses the HTTP_PROXY, HTTPS_PROXY, and NO_PROXY environmental variables in its start-up environment 
> to configure HTTP or HTTPS proxy behavior. You cannot configure these environment variables using the daemon.json file.

Create a file called `/etc/systemd/system/docker.service.d/https-proxy.conf` that adds the `HTTPS_PROXY` environment variable:

```
[Service]
Environment="HTTPS_PROXY=socks5://[USER]:[PASSWORD]@[HOST]:[PORT]/" "NO_PROXY=localhost,127.0.0.1"
```

Flush changes and restart Docker:

```
sudo systemctl daemon-reload \
&& sudo systemctl restart docker
```

Test:
```
docker pull hello-world
```
