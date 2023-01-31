---
title: Apache
layout: page
parent: Ubuntu
has_children: true
nav_order: 12
---
## Apache
`sudo systemctl start apache2.service`  
`sudo systemctl restart apache2.service`  

`cd /var/www/html/`  

`sudo systemctl status apache2.service -l --no-pager`  
`journalctl -xe`  

`sudo lsof -i:80`   
`sudo lsof -i:443`  
`sudo lsof -i -P -n | grep LISTEN` Все порты где есть прослушка  

`nano /etc/httpd/conf/httpd.conf`  
`nano /etc/apache2/ports.conf`

`sudo ufw app list`  Должен быть Apache. Если нет, то добавить:  
`sudo ufw allow 'Apache'`  
  
Если нет профиля: `sudo nano /etc/ufw/applications.d/apache2-utils.ufw.profile`  
```
[Apache]
title=Web Server
description=Apache v2 is the next generation of the omnipresent Apache web server.
ports=80/tcp

[Apache Secure]
title=Web Server (HTTPS)
description=Apache v2 is the next generation of the omnipresent Apache web server.
ports=443/tcp

[Apache Full]
title=Web Server (HTTP,HTTPS)
description=Apache v2 is the next generation of the omnipresent Apache web server.
ports=80,443/tcp
```

### Forbidden 403
На папке html
```bash
find ./ -type d -exec chmod 755 -R {} \;
find ./ -type f -exec chmod 644 {} \;
```

### Удаление
`sudo apt remove apache2`  
`sudo apt-get autoremove`  