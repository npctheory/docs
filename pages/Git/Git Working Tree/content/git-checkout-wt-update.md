## Git Checkout
`git checkout master` Обновить WT до состояние ласт-коммита master. Поставить HEAD на master.  
`git checkout --force master` Игнорировать изменения в текущей WT, обновить WT до состояния ласт-коммита master. Поставить на нее HEAD.  
`git checkout -f HEAD` Отменить все изменения WT, т.е. откатить её до ласт-коммита.  
`git checkout -f` Оменить все изменения WT, откатить её до HEAD.(HEAD это значение по умолчанию)

`git checkout @ index.html` Откатить index.html до состояния коммита, на котором HEAD.  
`git checkout @ -- index.html` то же самое  
`git checkout @~~~~` -- Program.cs //Откатить Program.cs на 4 коммита назад  
`git checkout @~4`  
`git checkout -` Переключиться на предыдующую ветку  