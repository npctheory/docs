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