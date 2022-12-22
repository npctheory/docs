## Git Checkout
`git checkout -b fix`  создает вершину fix, а если такая уже есть, ошибка? Двигает HEAD на fix.  

`git checkout -B feature 54a4` новая ветка feature создается на 54a4  
Эта команда делает то же, что  

`git branch -f feature` (создать вершину master на коммите, на котором стоит вершина, на которой стоит HEAD)  
и `git checkout feature` переместить HEAD на feature