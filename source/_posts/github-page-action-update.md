---
title: GitHub Pages 集成方式更新
date: 2023-07-24 11:26:17
tags:
  - github-actions
  - github-pages
categories:
  - 持续集成
top:
---

GitHub Pages 想必搭建个人博客的都不会陌生。这是一种十分友好的博客搭建方式，由全球最大的同性交友平台为您的博客保驾护航，还提供 github.io 的域名让你可以在任何地方直接访问。

<!-- more -->

## 前言

本站同样是使用 GitHub Pages 搭建，使用自定义域名映射到了自有域名上，使用时间已经超过三年，得力于这个全球性的平台，博客一直也是屹立不倒，就是压根没什么人看而已，包括我自己。

## 分支部署阶段

在上一个阶段，网站使用的是 GitHub Pages 提供的分支部署方式。

简单地说，只要你将整个静态网站推送到某个分支，然后在项目设置中设置要部署的分支即可。

![image-20230724125434804](https://images.orkva.com/images/2023/07/24/image-20230724125434804.png)

当然，由于绝大多数人都是使用的博客框架，然后经过各种工具构建成静态网站，所以古老的办法则是在本地进行构建，然后将构建好的静态网站上传至对应分支。

GitHub Actions 为我们提供了自动部署的方式，编写 `workflows` 工作流配置文件，便可以让 GitHub Actions 自动为你构建静态网站，下面以 Hexo 为例，文件保存在 `.github/workflows/pages.yml` 。

```yaml
name: Pages

on:  # 指定何时执行
  push:  # 指定 push 操作时执行
    branches:  # 指定 push 操作的分支
      - dev

jobs:
  pages:
    runs-on: ubuntu-latest  # 系统环境
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js lts
        uses: actions/setup-node@v3  # 使用 node 环境
        with:
          node-version: 'lts'  # node 版本
      - name: Cache NPM dependencies
        uses: actions/cache@v3  # 使用缓存
        with:
          path: node_modules  # 指定缓存路径
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3  # 部署到分支
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public  # 指定要推送的路径
          publish_branch: master  # 指定推送至哪个分支

```

这样便可以在推送原始文件到 dev 分支时，自动触发构建操作，并将构件好的静态网站推送至 master 分支，再结合之前提到的分支部署方式，就可以让你的网站发布在 `<username>.github.io` 。

## Actions 部署阶段

虽然当前还是 Beta 测试版本，但我还是想尝尝鲜。

GitHub Actions 部署是将静态网站打包为一个 github-pages 文件，存放在 Actions 的工作区，网站发布时直接从 Actions 获取数据。

```yaml
name: Deploy hexo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["dev"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# 执行脚本的权限
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js lts
        uses: actions/setup-node@v3
        with:
          node-version: 'lts/Hydrogen'
          cache: 'npm'
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

从使用结果来说，这种方式免去了在分支中管理网站的情况，毕竟静态网站只是源代码的附属产品，没必要维护在版本控制之中，对于一些爱好整洁的朋友还是相当不错的。
