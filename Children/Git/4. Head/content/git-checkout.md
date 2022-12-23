## Git Checkout
`git checkout master` Обновить WT до состояние ласт-коммита master. Поставить HEAD на master. Обычное переключение между ветками.  
`git checkout @{-1}`,`git checkout -` Вернуть HEAD на предыдущее значение   

`git checkout 1913` перевести HEAD на коммит 1913(без ветки) - в состояние detached head. Исправляется через `git checkout master` > `git-cherry-pick`