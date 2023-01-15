**6.3 вывод всего графа**  
### Git Log: вывод списка коммитов
`git log` Показать все коммиты от HEAD вниз
`git log master` Показать все коммиты от вершины master вниз  
`git log --oneline` номер, ветка, заголовок  
`git log --follow index.html`  все коммиты, где менялся index.html  
`git log --no-decorate` Не показывать бранчхеды и HEAD  
`git log --pretty=format:'%h %cd | %s%d [%an]'` \<id коммита\> \<дата\> | \<заголовок\> \<декорирование(ссылки)\> \[имя автора\]  

`git log --pretty=format:'%C(yellow)%h %C(dim green)%ad %C(reset)| %C(cyan)%s%d %C(#667788)[%an]' --date=format:'%F %R'`  
<details>
  <summary>Алиасы</summary>
  Алиас git lg
  <code>
  git config --global alias.lg 'log --pretty=format:'%C(yellow)%h %C(dim green)%ad %C(reset)| %C(cyan)%s%d %C(#667788)[%an]' --date=format:'%F %R'
  </code>
</details>

### Список коммитов от корня до коммита или бранчхеда: Git Log \<commit\>
`git log master` вывести все коммиты от корня до бранчхеда мастер  
`git log master feature` вывести все коммиты от корня до бранчхедов feautre и master  

### Список коммитов с regex-фильтром: Git Log --grep
Поиск по мессаджу  
`git log --grep добавил` найти слово коммиты со словом 'добавил'  
`git log --grep добавил -i` отключить регистрозависимость  
`git log --grep добавил --grep удалил` найти коммиты, где в мессадже есть или слово 'добавил' или слово 'удалил'  
`git log --grep добавил --grep удалил --all-match` найти коммиты, которые матчатся с обоими выражениями

Поиск по изменениям в файлах  
`git log -G"регулярноевыражения" -p`  
[документация](https://git-scm.com/docs/git-log#Documentation/git-log.txt--Gltregexgt)  
`git log -L`

`git config --global grep.patternType perl` Включить перловые regex

### История изменений внутри файла между строчками regex1 и regex2: Git Log -L    
`git log -L '/<head>/','<\/head>/':index.html` история изменений внутри тега head в index.html  
