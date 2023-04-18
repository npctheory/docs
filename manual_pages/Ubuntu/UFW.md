---
title: UFW
layout: page
parent: Ubuntu
nav_order: 11
---
{: .important }
Выяснить почему не работает IPV6=yes  

## UFW
[https://askubuntu.com/questions/541675/ufw-is-blocking-all-even-when-i-set-rules-to-allow]()  
`sudo ufw status verbose`  

`sudo iptables -L -v -n` Посмотреть iptables  
`sudo nano /etc/default/ufw` Конфигурация UFW (включить выключить IPV6=YES)  
`sudo nano /etc/ufw/sysctl.conf` Конфигурация UFW  

~~ufw reset~~ никогда не нажимать

`ufw enable` вкл  
`ufw disable` вкл 
`ufw Reload` 

### Базовая настройка UFW
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
### Дальше
Если надо удалить правило:
```bash
sudo ufw status numbered
```
```bash
sudo ufw delete 2
```
### IKEV2 Восстановление
```bash
ip route show default
```

```bash
sudo ufw allow 500,4500/udp
```
```bash
sudo nano /etc/ufw/before.rules
```  
Добавить перед *filter (интерфейс ens3)  
```
*nat
-A POSTROUTING -s 10.10.10.0/24 -o ens3 -m policy --pol ipsec --dir out -j ACCEPT
-A POSTROUTING -s 10.10.10.0/24 -o ens3 -j MASQUERADE
COMMIT

*mangle
-A FORWARD --match policy --pol ipsec --dir in -s 10.10.10.0/24 -o ens3 -p tcp -m tcp --tcp-flags SYN,RST SYN -m tcpmss --mss 1361:1536 -j TCPMSS --set-mss 1360
COMMIT
```
Добавить после #End required lines (секции *filters)
```
-A ufw-before-forward --match policy --pol ipsec --dir in --proto esp -s 10.10.10.0/24 -j ACCEPT
-A ufw-before-forward --match policy --pol ipsec --dir out --proto esp -d 10.10.10.0/24 -j ACCEPT
```
Дальше
```bash
sudo nano /etc/ufw/sysctl.conf
```
В конец файла  
```
net/ipv4/ip_forward=1
net/ipv4/conf/all/accept_redirects=0
net/ipv4/conf/all/send_redirects=0
net/ipv4/ip_no_pmtu_disc=1
```

```bash
sudo systemctl restart strongswan-starter
```  
#### Всё!  

### Проверка iptables
```bash
iptables -L -v -n
```

Поставить **IPV6=NO** а потом вернуть
```bash
sudo nano /etc/default/ufw
```


Удалить все правила UFW из iptables
```
for i in `iptables -L INPUT --line-numbers |grep '[0-9].*ufw' | cut -f 1 -d ' ' | sort -r `; do iptables -D INPUT $i ; done
for i in `iptables -L FORWARD --line-numbers |grep '[0-9].*ufw' | cut -f 1 -d ' ' | sort -r `; do iptables -D FORWARD $i ; done
for i in `iptables -L OUTPUT --line-numbers |grep '[0-9].*ufw' | cut -f 1 -d ' ' | sort -r `; do iptables -D OUTPUT $i ; done
for i in `iptables -L | grep 'Chain .*ufw' | cut -d ' ' -f 2`; do iptables -X $i ; done
```

Грохнуть iptables
```bash
iptables -F
```