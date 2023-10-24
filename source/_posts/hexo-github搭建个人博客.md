---
title: hexo+github搭建个人博客
date: 2023-10-24 12:39:05
tags: 
---

## 环境准备

install node.js

install git

```
git --version
```



## 流程搭建

### 安装hexo

新建一个文件夹，存放博客文件，如取名为/blog

在/blog文件夹下安装hexo

```shell
npm i hexo-cli -g
```

验证hexo是否安装

```shell
hexo -v
```

初始化文件夹

```shell
hexo init
```

安装必备组件

```shell
npm installs
```

生成静态网页

```shell
hexo g
```



### 本地部署

```shell
hexo s
```



### github远程部署

新建一个远程仓库

```
https://github.com/username/username.github.io.git
```

配置/blog下的_config.yaml中

```
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repository: https://github.com/username/username.github.io.git
  branch: main
```

远程部署

```
hexo d
```



### 主题选择



## 多端部署

### 原理简介

### 流程实现

新建一个branch并设置为默认分支,比如新分支叫hexo

克隆github仓库到本地，将原来的博客根目录下的文件拷贝过来（除.deploy_git, node_modules/, public/），

确认.gitignore内容

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

push源文件到远程仓库

```shell
git add .
git commit -m "add branch"
git push
```

在当前文件夹安装hexo，

```shell
npm install -g hexo-cli
hexo -v ## 验证下是否安装成功
npm install
```



## 问题汇总

* npm install -g hexo-cli 异常

```
$ npm install -g hexo-cli
npm ERR! code EEXIST
npm ERR! path C:\Users\Administrator\AppData\Roaming\npm\hexo.ps1
npm ERR! EEXIST: file already exists
npm ERR! File exists: C:\Users\Administrator\AppData\Roaming\npm\hexo.ps1
npm ERR! Remove the existing file and try again, or run npm
npm ERR! with --force to overwrite files recklessly.

npm ERR! A complete log of this run can be found in:
npm ERR!     C:\Users\Administrator\AppData\Local\npm-cache\_logs\2023-10-24T05_13_02_159Z-debug-0.log
```

这是由于之前可能装过hexo，需要在npm的appdata中删除原有的hexo modules



## 参考链接

[github+hexo搭建个人博客](https://zz2summer.github.io/github-hexo-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)

