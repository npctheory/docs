---
title: Docker advit
layout: page
parent: Docker
---
## Docker
`sudo systemctl status docker`  
`docker -v`  

## Команды
`docker run hello-world` скачает имидж hello-world и запустит 

`docker ps`  показать живые контейнеры
`docker ps -a`  показать и живые и отключенные контейнеры  
`docker images` показать все имиджи    

`docker search nginx` найти на докерхабе имидж tomcat  

`docker pull nginx` скачать но не запускать.

`docker run -d -p 1234:80 nginx`  запустить (скачать если не скачан) как демон  
`docker run -it -p 1234:80 nginx`  запустить интерактивно  
`-p 1234:8080` Чтобы обратиться к порту 8080 контейнера докер, надо обратиться к порту 1234 машины.  

`docker stop e2e4` остановить  
`docker rm e2e4` грохнуть контейнер  
`docker rmi nginx` удалить имидж (контейнеры должны быть остановлены)  

`docker build -t myimage:v1 .` создать имидж в текущей директории  

`docker tag myimage:v1 myimage:v2` переименовать тег  

`docker exec -it e2e4 /bin/bash` залогиниться в уже запущенный контейнер и запустить bash  

### Dockerfile (docker build)
`docker build -t testimage:v1 .`  сделать имидж из Dockerfile в текущей папке  
`docker run -d -p 1377:80 testimage:v1` запустить  

`nano ~/mydocker/Dockerfile`  
```dockerfile
FROM ubuntu:20.04

RUN apt-get -y update
RUN apt-get -y install apache2

RUN echo `Hello World from Docker! > /var/www/html/index.html`

CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]
EXPOSE 80
```



### Установка
```bash
sudo apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

sudo apt install apt-transport-https

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```  
`sudo usermod -aG docker $USER` Добавить права текущему юзеру  
