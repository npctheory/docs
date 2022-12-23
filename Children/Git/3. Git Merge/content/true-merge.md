### Истинный мердж
Когда один бранчхед не входит в ветку другого. HEAD стоит на master, мердж с feature.
`git status` должен быть пустой.  
`git diff --name-only master feature` покажет какие файлы различаются.  
`git merge-base master feature` покажет на каком коммите была развилка.  
`git merge feature`  анализирует изменение в каждом файле. Пробует auto-merge  

### CONFLICT
Если мердж прервался
`cat .git/MERGE_HEAD` покажет id коммита, с которым мы мерджились, но возник конфликт.  
`git show e2e4:index.html`  
`git show master:index.html`  
`git show feature:index.html`  
покажут версии файлов в разных бранчхедах.

### 7.1-7.2
`git checkout --ours`  
`git checkout --theirs`  
`git checkout --merge`  
`git merge --abort`  
`git checkout --conflict=diff3 --merge index.html`  
`git merge --continue`  
