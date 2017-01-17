---
title: hexo 3 安装简单指北
date: 2017-01-16 20:05:36
tags: hexo
---

之前装过一次 hexo ，用了一段时间，后来重做系统之后，安装出现了一些问题，花了一些时间重新整理一下。

<!-- more -->

node.js 和 git 安装好后，开始安装 hexo 。

```shell
sudo npm install hexo -g
```

### 1. 初始化

```shell
hexo init hexo
```
网上有教程说先创建目录，再执行
```
hexo init
```
也是可以的。

这步执行完就可以执行 clean ，generate ， server 等命令了。

### 2. 部署
如果要使用 github 提供的网站服务，就需要将 hexo 部署到 github.io 上，所以需要先安装部署插件。

```shell
npm install hexo-deployer-git --save
```
配置 git
```
deploy:
  type: git
  repository: git@github.com:bsyonline/bsyonline.github.io.git
  branch: master
```

```shell
heox clean
hexo g
hexo deploy
```
至此，访问 bsyinline.github.io 就可以看到网页。
