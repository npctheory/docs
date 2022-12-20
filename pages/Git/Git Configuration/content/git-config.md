## Конфигурация Git

### Задать имя и почту в репозитории локально (в файле .git/config)  
`git config user.name "Name Surname"`
`git config user.email mail@example.com`  
`cat .git/config` Вывест конфиг текущей папки (через cat)  
`git config --list` Вывести конфиг репозитория текущей папки (через git)  
Удалить из репозитория текущей папки имя и почту:  
`git config --unset user.name`  
`git config --unset user.email`  
`git config --remove-section user`  

### Задать имя и почту глобально  
`git config --global user.name "..."`  
`git config --global user.email "..."`  
`git config --system core.autocrlf true` Найстроить лайн-брейки на уровне системы
`cat ~/.gitconfig` Вывести глобальный конфиг (через cat)  
`git config --list --global` Вывести глобальный конфиг (через git) 
`git config --global -e` Открыть конфиг в редакторе по умолчанию  

`git config --global core.editor /Applications/Sublime\` Текстовый редактор по умолчанию  
[Примеры для разных редакторов](https://docs.github.com/en/get-started/getting-started-with-git/associating-text-editors-with-git)  
`git config --global core.editor "code --wait"`  VsCode

### Каскад от более к менее приоритетному:  
`--local`  
↓  
`--global`  
↓  
`--system`  

### Алиасы git
`git config  --global alias.c 'config --global'`  
`git c` вместо `git config --global`  
