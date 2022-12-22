## Git Branch
`git branch feature` Создать на последнем коммите вершину feature  
`git branch --force feature 54a4` Создать или подвинуть если уже есть вершину feature на коммит 54a4.  
`git branch feature HEAD@{6}` создать вершину feature на шестом положении HEAD из рефлога  
`git branch feature HEAD@{'2017-09-12 22:49:07 +0200'}` восстановить вершину feature на состояние HEAD из даты  
