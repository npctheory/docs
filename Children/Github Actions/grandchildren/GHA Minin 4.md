---
title: Github Actions Minin 4
layout: page
parent: Github Actions Tutorials
grand_parent: Github Actions
repourl: https://github.com/aq-coding/github-actions/
---
### Артифакты
Артифакты это билды и тест-репорты  
Существует отдельный экшн
[Upload a Build Artifact](https://github.com/marketplace/actions/upload-a-build-artifact)  
[Download A Build Artifact](https://github.com/marketplace/actions/download-a-build-artifact)  
```yaml
name: Build
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - name: Get code
          uses: actions/checkout@v3
        - name: Install deps
          run: npm ci
        - name: Build project
          run: npm run build
        - name: Upload Artifact
          uses: actions/upload-artifact@v3
          with:
            path: build
            name: build-files
    deploy:
      needs: build
      runs-on: ubuntu-latest
      steps:
        - name: Get build project
          uses: actions/download-artifact@v3
          with:
            name: build-files
```

### Environment, Секреты
```yaml
name: Environment
on:
  push:
  workflow_dispatch:
env:
  NODE_ENV: production
  GH_SECRET: 42
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Print Env Build
      run: |
        {% raw %}echo "${{ env.NODE_ENV}}"
        echo "${{ env.GH_SECRET}}"{% endraw %}
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Print Env Deploy
      run: |
        {% raw %}echo "${{ env.NODE_ENV }}"
        echo "${{ env.GH_SECRET }}"{% endraw %}
```

Секреты задаются в настройках репозитория.  
Один раз настроив, его уже нельзя будет прочитать.  
