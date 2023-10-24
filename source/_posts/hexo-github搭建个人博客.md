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

在 Hexo 中有两份主要的配置文件，其名称都是 `_config.yml`。 其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。

为了描述方便，在以下说明中，将前者称为 **站点配置文件**， 后者称为 **主题配置文件**。

Hexo 安装主题的方式非常简单，只需要将主题文件拷贝至站点目录的 `themes` 目录下， 然后修改下配置文件即可。具体到 NexT 来说，安装步骤如下。

在终端窗口下，定位到 Hexo 站点目录下。使用 `Git` checkout 代码：

```shell
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

启用主题

与所有 Hexo 主题启用的模式一样。 当 克隆/下载 完成后，打开 **站点配置文件**， 找到 `theme` 字段，并将其值更改为 `next`。

```
theme: next
```

验证主题

```
hexo d -g
```

配置主题配置文件



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

* hexo主题切换导致乱码

```
{% extends '_layout.swig' %} {% import '_macro/post.swig' as post_template %}....................
```

hexo在5.0之后把swig给删除了需要自己手动安装

```
npm i hexo-renderer-swig
```

重新生成

```
hexo clean          
hexo generate      
hexo server
```



## 参考链接

* [github+hexo搭建个人博客](https://zz2summer.github.io/github-hexo-%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)

* [hexo主题切换乱码问题解决](https://www.cnblogs.com/lanhualan/p/14588669.html)

* [next主题配置](http://theme-next.iissnan.com/getting-started.html)

* [hexo跨平台搭建](https://dora-cmon.github.io/posts/454ba26/)
