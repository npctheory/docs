---
title: Github Actions Minin 3
layout: page
parent: Github Actions Tutorials
grand_parent: Github Actions
repourl: https://github.com/aq-coding/github-actions/
---
### Contexts & Expressions
В папка **.github/workflows**  
создать **pull_request.yml**  


Объект контекста, который называется \{\{github\}\}

pull_request.yml
```yaml
name: Print Pull Request Context
#Запуск либо по кнопке либо по пулл-реквесту
on:
  workflow_dispatch:
  pull_request:
    types: [opened, edited, reopened]
jobs:
  print:
    runs-on: ubuntu-latest
    steps:
      - name: Print context  
        run: echo $"{{ "{{ toJSON(github.event) " }}}}"
```

### Зауск только при пуше в master или изменения в какой-то папке
deployment.yaml  
```
on:
push:
  branches:
    'master'
  paths:
    - '**.js'
jobs:
```

### Pull request,
Перейти на вкладку [Pull Requests]({{ page.repourl }}pulls)  

Выбрать [New pull Request](#){: .btn .btn-green }  

Выбрать **base:master** <- **compare:dev**  

Выбрать [Create pull Request](#){: .btn .btn-green }  

### paths-ignore
Не выполнять джоб если изменения в определенных файлах  
```yaml
on:
  push:
    paths-ignore:
      - '.github/workflows/*'
```

### Marketplace: Cache
`uses: actions/cache@v3`  

```yaml
steps:
  - name: Get repository code
    uses: actions/checkout@v3
  - name: Cache deps
    uses: actions/cache@v3
    with:
      path: ~/.npm
      key: node-modules-deps
  - name: Install dependencies
    run: npm ci
  - name: Lint application
    run: npm run lint
```
В key можно считать хэш с lock-файла  
key: node-modules-$\{\{ hashFiles('**/package-lock.json') \}\}  

### Матрицы
Параллельно протестировать билды на 14 и 16 ноде и на ubuntu-latest и на windows
```yaml
strategy:
  matrix:
    node-version: [14, 16]
    os: [ubuntu-latest, windows-latest]
```  
После этого в runs-on можно записать:  

`runs-on:`
$\{\{ matrix.os \}\}

steps:
  - name: Install Node Js  
  uses: actions/setup-node@v3  
  with:  
    node-version: $\{\{ matrix.node-version \}\}  

Разрешить продолжать работаеть если один из элементов матрицы закончился с ошибкой:  
```yaml
build:
  continue-on-error: true
```