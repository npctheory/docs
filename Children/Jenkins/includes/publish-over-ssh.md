## Publish over SSH
### Добавление удаленного сервера
Manager Jenkins > Configure Systems  
Publish over SSH  

Key  
Name: `Любое имя`  
Hostname: `ip-адрес`  
Username:  
Remote directory: `/var/www/html`  

<span class="fs-1">[Test Configurations](#){: .btn }</span>  
### Настройки шага джоба
**Post-build Action**  
Send build artifacts over SSH:  
Source files  `/*`  
Remote directory  
Exec command  `sudo service httpd restart`  