---
title: AdvitGit
layout: page
parent: Git
---
Перед началом работы всегда делается `git pull`
### Работа с локальным репозиторием
```bash
git status
git add *
git commit -m "Initial"
```
### История
```
echo "aa" > file.txt # Создать новый файл с текстом аа
echo "aa" >> file4.txt # Добавить в конец файла текст аа
```
##### Показать изменения во всех файлах в предыдущем коммите
`git log`
`git log -p` ???
`git log -1 -p`  ???
##### Вернуть файл в состояние до изменения. Чтобы git status показывал "no changes"
`git checkout -- file1.txt`  
##### Посмотреть что было добавлено через git add в current commit
`git diff --staged`  
### Github SSH
Создать репозиторий через WebGUI гитхаба.  
```bash
git clone ....git
echo "HelloWorld" > file1.txt
git add .
git commit -m "My first"
git push origin
```
### Ветки
В master коммиты не делаются. Только merge.  
```bash
git init myapp
git add .
git commit -m "message"
git branch develop #Создание бранчхеда fix_error
git checkout develop #Передвинуть HEAD на бранчхед fix_error
```
### Полный рабочий цикл
[Ссылка](https://www.youtube.com/watch?v=m3voEkMZLvA&list=PLg5SS_4L6LYstwxTEOU05E0URTHnbtA0l&index=13)  
Ветку обычно называют имя-разрабочика_номер-тикета
```bash
git checkout -b aq_task001
#Работа с проектом...
git add .
git commit -m "Added changes"
git push --set-upstream origin aq_task001
#Со своего гитхаба нажимем кнопку Compare & pull request
#Пишем коммент Create Pull Request
```
Сеньор делает Merge pull request
Удаляется бранч локально `git branch -d aq_task001`
Удаляется бранч удаленно `git push origin --delete aq_task001`
### Tags
`git tags`  
`git tag v1.0.1` Создать тег  
`git tag -d v1.0.1` удалить тег локально  
`git push origin --delete v1.0.1` удалить удаленно  
