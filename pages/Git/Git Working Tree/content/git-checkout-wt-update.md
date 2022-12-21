## Git Checkout
Обновляет файлы/папки WT и двигает HEAD на указанный коммит.  
**Если указан путь к файлам/папкам, HEAD никуда не двигается.**  
`git checkout master` Обновить WT до состояние ласт-коммита master. Поставить HEAD на master.  
`git checkout --force master` Игнорировать изменения в текущей WT, обновить WT до состояния ласт-коммита master. Поставить HEAD на master.  
`git checkout -f HEAD` Отменить все изменения WT, до состояние коммита master.  Используется, чтобы отменить всё, что было сделано для текущего коммита.    
`git checkout -f` Отменить все изменения WT, откатить её до HEAD.(HEAD это значение по умолчанию).

`git checkout @ index.html` Откатить index.html до состояния коммита, на котором HEAD.  
`git checkout @ -- index.html` то же самое  
`git checkout @~~~~` -- Program.cs //Откатить Program.cs на 4 коммита назад  
`git checkout @~4`  
`git checkout -` Переключиться на предыдующую ветку  