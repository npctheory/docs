---
title: Docker Tim Corey
layout: page
parent: Docker
---
Создать **index.md**  
В той же папке **создать Dockerfile**  

```dockerfile
# COPY ./html/ Скопировать всё из папки html и имидж, в папку /usr/local/apache2/htdocs/
FROM httpd:alpine
COPY ./html/ /usr/local/apache2/htdocs/
```

имидж - это ридонли билд  
`docker build -t hello-docker:1.0.0 .`  
`docker image history e2e4` лог процесса билда имиджа по шагам  

`docker run --name first-container -p 8080:80` запустить контейнер 8080 внутри - 80 внутри  
`docker stop e2e4` остановить контейнер
`docker rm` удалить контейнер

После изменения в файлах надо создавать новый имидж. Можно оставить название и поменять версию.  

`docker build -t hello-docker:1.0.1 .`  
`docker run -d --name first-container -p 8081:80 hello-docker:1.0.0`  
`docker run -d --name second-container -p 8080:80 hello-docker:1.0.1`  

### RabbitMQ
`docker run -d --hostname my-rabbit --name some-rabbit -p 8080:15672 rabbitmq:3-management`  
`-d` disconnected  
`--hostname` нужно для rabbitmq  
`--name` имя контейнера  
`-p` `8080` снаружи для порта `15672` внутри  
`:3-management` имидж с rabbitmq и менеджмент-интерфейсом  

Логин: guest  
Пароль: guest  

`docker pull mysql`