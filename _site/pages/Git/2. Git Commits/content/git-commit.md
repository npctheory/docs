## Git Commit
`git commit` Отправить в репозиторий текущий коммит  
`git commit --author='John Smith' <john@me.com> --date='...'` Задать автора, дату  
`git commit -m "Text"` Закоммитить с сообщением "Text"  
## Git Commit+Add
`git commit -m 'Text' index.md` Добавить изменения файла index.md в индекс, закоммитить с текстом Text. Работает только для tracked файлов.   
`git commit -a` Добавить в индекс все tracked файлы  
`git commit -a -m 'Text'` то же, добавить в индес все modified и закоммитить.  
`git commit -am 'Text'` -||-
<details>
<summary>Алиас для честного Git Add . вместе с Git Commit</summary>
<code>
git config --global alias.commitall '!git add .;git commit'  
<br>
git config --global alias.commitall '!git add -A;git commit' 
</code><br>  
Вызовы будут выглядеть:<br>
<code>git commitall -m 'Text'</code>
</details>
