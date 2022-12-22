## Создать ветку для промежуточного результата
```
git checkout -b fix
git commit -m "Промежуточный результат"
git checkout master
```

## Подвинуть вершину master на несколько коммитов назад, а текущую ветку сделать fix
`git branch fix`  
`git branch --f master 54a4` Всё, что осталось выше 54a4 теперь в ветке fix  
или  

`git checkout -B master 54a4` новая ветка master создается на 54a4
делает то же,  
что `git branch -f master`; `git checkout master`