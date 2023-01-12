---
title: Git Flow
layout: page
parent: Github
---
[Ссылка](https://www.youtube.com/watch?v=umiT0yIsSrc)  
### Git Flow
main
release
develop
feature/1, feature/2, feature/3
bug/1, bug/2, bug/3

 - Две ветки:  
   Main коммиты в нее делать нельзя  
   Develop коммиты в них делать нельзя  
 - Ветки, в которые делают коммиты
   Feature/Название_задачи  
   Bug/Название_задачи
 - Из Feature и Bug коммиты попадают в Develop через Pull Request
 - Из Develop коммиты попадают в Release/R1
 - Из Release попадают в Main. В Main на коммиты ставятся теги.
 - Иногда из Main созда
### Github FLow
Лучше подходит для микросервисов
main
feature