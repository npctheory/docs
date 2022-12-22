## Git Reset \-\-soft
Ресетит вершину бранчи, на которой сейчас HEAD, HEAD двигается вместе с вершиной.  
`git reset --soft 9e5e` Подвинуть вершину бранчи и вместе с ней HEAD, в коммит 9e5e  
`git reset --soft HEAD~` Подвинуть вершину бранчи и вместе с ней HEAD на один коммит назад

## Git Reset \[\-\-mixed\] (по умолчанию)
То же что \-\-soft, и изменяет текущий коммит/Index, записывая в него то, что было в указанном коммите  
`git reset 9e5e` Подвинуть вершину бранчи, HEAD, изменить Index на коммит 9e5e  
`git reset 9e5e --mixed` -||-  
`git reset HEAD` Откатить индекс до состояние прошлого коммита. Использовать после `git add .` чтобы отменить лишнее проиндексированное  
`git reset HEAD .idea`  Убрать из staginng файлы .idea  

## Git Reset \-\-hard
`git reset --hard 2fad` Отменить всё сделанное после коммита 2fad - вернуть текущую ветку\[master\] и HEAD вместе с ней на 2fad. Если запомнить номер коммита (найти через `cat .git/ORIG_HEAD` или в `git reflog`) можно вернуть master и HEAD обратно так же чере `git reset --hard`.  

`git reset --hard HEAD`