## Fast-Forward
`git merge fix` Если ветка fix выше чем ветка 'master', на которой стоит HEAD - подвинет master вместе с HEAD на коммит fix. fix не удаляется. 
Дальше можно переместить HEAD на fix через `git checkout fix`

`cat .git/ORIG_HEAD` покажет id предыдущего положения HEAD  
`git branch -f master ORIG_HEAD` вернуть master где она была
