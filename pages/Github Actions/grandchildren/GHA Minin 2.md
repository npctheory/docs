---
title: Github Actions Minin 2
layout: page
parent: Github Actions Tutorials
grand_parent: Github Actions
repourl: https://github.com/aq-coding/github-actions/
---
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

### Последовательно, в одном джобе

```yaml
name: Test React App
on: push
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Get repository code
        uses: actions/checkout@v2.6.0
      - name: Install dependencies
        run: npm ci
      - name: Test application
        run: npm run test
      - name: Lint application
        run: npm run lint
      - name: Build application
        run: npm run build
      - name: Deploy
        run: echo Deploying...
```