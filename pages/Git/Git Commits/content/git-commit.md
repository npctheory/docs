## Git Commit
`git commit` Отправить в репозиторий текущий коммит  
`git commit --author='John Smith' <john@me.com> --date='...'` Задать автора, дату  
`git commit -m "Text"` Закоммитить с сообщением "Text"  
## Git Commit+Add
`git commit -m 'Text' index.md` Добавить изменения файла index.md в индекс, закоммитить с текстом Text. Работает только для tracked файлов.   
`git commit -a` Добавить в индекс все tracked файлы  
`git commit -a -m 'Text'` то же, добавить в индес все modified и закоммитить.  
`git commit -am 'Text'` -||-
## Алиас для честного Git Add . вместе с Git Commit
`git config --global alias.commitall '!git add .;git commit'`
`git config --global alias.commitall '!git add -A;git commit'`
Вызовы будут выглядеть `git commitall -m 'Text'`




`git commit --ammend` = `git reset --soft` + `git commit` 