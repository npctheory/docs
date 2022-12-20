## Git Add
`git add .` Добавить все untracked файлы в текущий коммит  
`git add -A`  То же, но для тех ОС, где нет оператора точки
`git add src` Добавить папку в текущий коммит  
`git add index.md` Добавить index.md в текущий коммит  
`git add -p index.html index.md index.php` git будет спрашивать что делать с каждым измененным файлом  

Добавить файлы, которые под .gitignore:  
`git add --force`, `git add -f`
