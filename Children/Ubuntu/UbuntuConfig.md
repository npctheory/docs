---
title: UbuntuConfig  
layout: page  
parent: Ubuntu  
---
### Базовая настройка UFW и IEv2
```
sudo apt update
sudo apt install strongswan strongswan-pki libcharon-extra-plugins libcharon-extauth-plugins
```
```
mkdir -p ~/pki/{cacerts,certs,private}
chmod 700 ~/pki
```

Проверить чтобы стояло IPV6=yes  
```bash
sudo nano /etc/default/ufw
```

Запретить все входящие, разрешить все исходящие  
```bash
sudo ufw default deny incoming
```
```bash
sudo ufw default allow outgoing
```
Разрешить входящие ssh  
```bash
sudo ufw allow ssh
sudo ufw allow 500,4500/udp
sudo ufw allow 7777/tcp
sudo ufw allow 8080/tcp
sudo ufw allow 9418/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```
Включить  
```bash
sudo ufw enable
```