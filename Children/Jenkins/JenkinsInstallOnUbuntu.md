---
title: Установка Jenkins на Ubuntu
layout: page
parent: Jenkins
nav_order: 1
pqip: "45.89.52.66"
---
[Прямая ссылка на http://{{ page.pqip }}:8080/login](http://{{ page.pqip }}:8080/login)  

[Инструкция DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-20-04)  
[Инструкция с оф сайта Jenkins](https://pkg.jenkins.io/debian-stable/)
### Проверить версии софта на сервере
`cat /etc/os-release` версия ОС  

`java -version` версия Java. Должна быть 11 ?

`sudo lsof -i:8080` занят ли порт 8080. Должен быть свободен. `apt remove apache2` если там апач    

`grep ^ /etc/apt/sources.list /etc/apt/sources.list.d/*` Показать sources.list . Репозиторий уже мог быть добавлен. [More](https://askubuntu.com/questions/148932/how-can-i-get-a-list-of-all-repositories-and-ppas-from-the-command-line-into-an)  

### Обновление локальных пакетов
`sudo apt upgrade`  
`sudo apt update`  
`sudo apt install ca-certificates`  

### Установка Java
Удалить всю старую джаву. [More](https://askubuntu.com/questions/84483/how-to-completely-uninstall-java)  
`dpkg-query -W -f='${binary:Package}\n' | grep -E -e '^(ia32-)?(sun|oracle)-java' -e '^openjdk-' -e '^icedtea' -e '^(default|gcj)-j(re|dk)' -e '^gcj-(.*)-j(re|dk)' -e '^java-common' | xargs sudo apt-get -y remove`  
`sudo apt-get -y autoremove`

Установить с нуля Java 11  
`sudo apt-get install openjdk-11-jdk`  
`sudo apt-get install fontconfig`  

Добавить путь к Java  
`nano /etc/environment` Прописать `JAVA_HOME="/usr/lib/jvm/java-1.11.0-openjdk-amd64"`  
`source /etc/environment` Перезагрузить  
`echo $JAVA_HOME` Проверить всё ли работает  

### Добавление репозитория jenkins-stable binary
`wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -` Добавление ключа репозитория 

`sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'` Добавление в sources.list  


### Собственно установка
`sudo apt install jenkins`  

### UFW
Проверить, запущен ли UFW  
`sudo ufw status`  
<details>
<summary>Ответ должен быть: Когда UFW включен, 8080 открыт, OpenSSH включен</summary>
<pre>Status: active

To                         Action      From
--                         ------      ----
8080                       ALLOW       Anywhere
OpenSSH                    ALLOW       Anywhere
8080 (v6)                  ALLOW       Anywhere (v6)
OpenSSH (v6)               ALLOW       Anywhere (v6)</pre>
</details>

Если пустые правила, надо открыть 8080 (Jenkins по умолчанию) и добавить OpenSSH  
`sudo ufw allow 8080`  
`sudo ufw allow OpenSSH`  
`sudo ufw enable`  

`sudo service jenkins start` Запуск  
`sudo service jenkins stop` Остановить  
`sudo service jenkins status` Статус. должно быть active(running)  
`sudo systemctl status jenkins.service` Статус. должно быть active(running)  

`curl localhost:8080` должно работать  
`netstat -ntulp | grep 8080`  
`sudo lsof -i:8080` Кто слушает порт 8080  

### Первый запуск
[Прямая ссылка на http://{{ page.pqip }}:8080/login](http://{{ page.pqip }}:8080/login)  
`cat /var/lib/jenkins/secrets/initialAdminPassword`  выставить.
Установить Suggested.  

### Настройка языка
[Плагин Locale](https://plugins.jenkins.io/locale/)  
Dashboard->Настроить Jenkins->Конфигурация системы->Ctrl+F "Locale"
Default Language: `en`  
