---
title: Manage Jenkins
layout: page
grand_parent: Jenkins
parent: Jenkins Installation
nav_order: 2
jenkinsroot: "http://45.89.52.66:8081/"
---
### Configure System
\# of excutors: **4**  
Usage: только .net
Jenkins URL: **Поставить 8081**  
Usage Statistics: **Отключить**  
Save  
### Configure Global Security
Authorization:  
**Logged-in users can do anything**  
- [x] Allow anonymous read access  

### Установка обновления
`cd /usr/share/jenkins`  
Скачать в эту папку  
`sudo mv jenkins.war jenkins_новая_директория`  
`ll`  
`sudo service jenkins restart`  
