---
title: Marketplace
layout: page
parent: Github Actions
nav_order: 1
---
[Checkout](https://github.com/marketplace/actions/checkout)  
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
```
```yaml
      - name: Checkout
        uses: actions/checkout@v3
```