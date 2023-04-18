---
title: Github Actions Minin
layout: page
parent: Github Actions Tutorials
grand_parent: Github Actions
repourl: https://github.com/aq-coding/github-actions/
---
### Создание репозитория
**index.html**  

`git init`  
`git add .`  
`git commit -m "init"`  

Создать на гитхабе репозиторий **github-actions**  
`git remote add origin https://github.com/aq-coding/github-actions.git`  
`git push -u origin master`

Если не работает git push:  
`git remote set-url origin https://новый_токен@github.com/aq-coding/github-actions.git`  
`git push -u origin master`

### Страница создания workflow
[{{ page.repourl }}actions/new]({{ page.repourl }}actions/new)  

Выбрать
**set up a workflow yourself**  
github-actions./github/workflows/**demo.yml** in master  
```yaml
#Имя любое
name: Demo Workflow
#on какое событие слушать
# Ctrl + space список ивентов
on: workflow_dispatch
jobs: 
  #Имя работы любое
  print:
    # Выбор раннера
    runs-on: ubuntu-latest
    # Внутри джоба print расписываем шаги:
    steps:
      #Имя шага
      - name: Print to console
        run: echo Hello GH Actions!
```

### Запуск workflow demo.yml  
Actions > Demo Workflow
[{{ page.repourl }}actions/workflows/demo.yml]({{ page.repourl }}actions/workflows/demo.yml)  
[Run Workflow](#){: .btn }

### Отработавшие workflows  
Actions > All Workflows
[{{page.repourl}}actions]({{page.repourl}}actions)  

### Изменить workflow
Actions > Demo Workflow  
[View workflow file](#){: .btn }

## Github Actions React
`git remote add origin https://github.com/aq-coding/github-actions-react.git`  
`git push -u master`  

Создать руками папку **.github/workflows**  
Создать руками **testing.yml**  

### Marketplace: Actions
[Marketplace/Actions](https://github.com/marketplace?category=&query=&type=actions&verification=)  
[Use the latest version](#){: .btn .btn-green }  Скопировать  
```yaml
- name: Checkout
        uses: actions/checkout@v2.6.0
```

### Несколько джобов параллельно
```yaml
name: Test React App
# Триггер на каждый новый залив
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm install
      - name: Test application
        run: npm run test
  lint:
    runs-on: ubuntu-latest
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm ci
      - name: Test application
        run: npm run lint
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm ci
      - name: Test application
        run: npm run build
      - name: Deploy
        run: echo Deploying...
```


### Сначала test, потом lint, потом build
```yaml
name: Test React App
# Триггер на каждый новый залив
on: push
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm install
      - name: Test application
        run: npm run test
  lint:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm ci
      - name: Test application
        run: npm run lint
  build:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm ci
      - name: Test application
        run: npm run build
      - name: Deploy
        run: echo Deploying...
```