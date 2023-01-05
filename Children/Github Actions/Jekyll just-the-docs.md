---
title: Jekyll just-the-docs
layout: page
parent: Github Actions
nav_order: 2
---
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
```

[GitHub Action: Checkout](https://github.com/marketplace/actions/checkout)  
```yaml
      - name: Checkout
        uses: actions/checkout@v3
```

[GitHub Action: Setup Ruby, JRuby and TruffleRuby](https://github.com/marketplace/actions/setup-ruby-jruby-and-truffleruby)  
```yaml
      - name: Setup Ruby, JRuby and TruffleRuby
        uses: ruby/setup-ruby@v1.133.0
        with:
          ruby-version: '3.1'
          bundler-cache: true
```

[GitHub Action: Configure GitHub Pages](https://github.com/marketplace/actions/configure-github-pages)  
```yaml
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v2.1.3
```

RUN  
Сгенерить html-файлы в папку _site
```yaml
      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
```

[GitHub Action: Upload GitHub Pages artifact](https://github.com/marketplace/actions/upload-github-pages-artifact)  
Automatically uploads an artifact from the './_site' directory by default
```yaml
      - name: Upload GitHub Pages artifact
        uses: actions/upload-pages-artifact@v1.0.7
```