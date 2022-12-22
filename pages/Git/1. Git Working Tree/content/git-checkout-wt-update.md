## Git Checkout
Всё, что после `--` воспринимается как путь.  
`git checkout e2e4 -- <treeish>` Откатывает состояние в WT файла до состояния в коммите e2e4, и сразу добавляет новое состояние файла в индекс.  
 `git reset -- <treeish>` Сбрасывает состояние файла в индексе до состояние в HEAD.  

## Откат всей WT через Git Checkout \<branch\>. Перемещает HEAD  
`git checkout -f` Отменить все изменения WT, отменить весь текущий коммит. untracked не трогает.  
`git checkout -f HEAD` То же самое.   

`git checkout master` Обновить WT до состояние ласт-коммита master. Поставить HEAD на master.  
`git checkout --force master` Игнорировать изменения в текущей WT, обновить WT до состояния ласт-коммита master. Поставить HEAD на master.  

## Откат отдельных файлов до состояния коммита через Git Checkout
`git checkout 54a4 -- index.html` Откатить index.html до 54a4 + Добавить новую версию в индекс. То же что git checkout 54a4 index.html  
`git reset -- index.html` Отмена индексирования index.html, будет возвращен в состояние HEAD.  

`git checkout @ index.html` Откатить index.html до состояния коммита, на котором HEAD.  
`git checkout @ -- index.html` то же самое  
`git checkout @~~~~` -- Program.cs //Откатить Program.cs на 4 коммита назад  
`git checkout @~4`  
`git checkout -` Переключиться на предыдующую ветку  