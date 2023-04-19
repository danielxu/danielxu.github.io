---
title: >-
  在 macOS Ventura 13.3.1 for M2 芯片上搭建基于 github Pages的博客系统
comments: false
date: 2023-04-19 14:00:16
tags:
  - 教程
  - Hexo
  - macOS
  - Venturan
  - M2 Pro
  - github
  - pages
categories: 教程
keywords: 'macOS,Ventura,13.3.1,M2 Pro,Hexo,教程'
description: 在 macOS Ventura 13.3.1 for M2 芯片上搭建基于 github Pages的博客系统
top_img:
cover:
sticky: 100
---

{% note blue 'fas fa-bullhorn' %}

 之前使用的是 octopress, 由于年久且不在 Apple M2 芯片上支持（ruby 1.9 版本，未在系统上安装成功），所以使用 Hexo 进行替代

{% endnote %}

***

[Hexo](https://hexo.bootcss.com/) 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

## 安装前

安装 Hexo 之前，先确保已经安装下列应用程序即可：
* [Node.js](http://nodejs.org/) (Node.js 版本需不低于 10.13，根据安装插件实验，最好是 v14.21.3 以上)
* [Git](http://git-scm.com/)

> 安装 Node 建议使用 [nvm](https://github.com/nvm-sh/nvm)
> 安装 Git ，在 MacOS 系统中安装  Xcode Command Line Tools，在命令行中输入 `git --version ` 如果没有安装过命令行开发者工具，将会提示你安装。
> 如果编译过程中提示缺少组件，使用 [Homebrew](https://brew.sh/) 安装 `brew instal 组件`

## 安装

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。
```bash
npm install -g hexo-cli
```

**安装一键部署 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)**

```bash
npm install hexo-deployer-git --save
```

修改配置
```yaml
deploy:
  type: git
  repo: <repository url> # 如 git@github.com:用户名/project名
  branch: gh-pages
```

## github 上的操作

创建一个公共仓库，如：xxxx.github.io，然后按照 github 的指引，初始化仓库。

切换到 Settings 选项卡，在 Security - Deploy keys 里添加 ssh key，选中写权限。
如果本机还没生成，使用如下命令生成并复制 
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

pbcopy < ~/.ssh/id_rsa.pub
```

## 建站

安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
```bash
hexo init <folder>
$ cd <folder>
$ npm install
```

**_config.yml**

网站的[配置](https://hexo.bootcss.com/docs/configuration.html)信息
```yaml
# Site
title: xxxx
subtitle: ''
description: ''
keywords:
author: xxx
language: zh-CN
timezone: 'Asia/Shanghai'

# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io'
url: 
```

## 部署

使用 new 命令创建一篇文章，然后 generate 生成静态文件，再 deploy 到 github

```bash
hexo new post test
hexo generate
hexo deploy
```

推送成功后，上面配置的 github 项目会多出配置的 gh-pages 分支，再到 github 代码库的 Settings - Pages 里，Build and deployment - Source 选择 Deploy from a branch，然后 Branch 选择刚才生成的 gh-pages，
Select folder 选择 root。保存，一会就能在 https://username.github.io 看到你的站点了。

> Hexo 具体操作，请才考 https://hexo.bootcss.com/docs/

## 主题

默认主题生成的文章，显示有点问题，我这里选择了主题 [butterfly](https://github.com/jerryc127/hexo-theme-butterfly)

*** 

`hexo-theme-butterfly` hexo-theme-butterfly 是基于 [hexo-theme-melody](https://github.com/Molunerfinn/hexo-theme-melody) 的基础上进行开发的。

在你的 Hexo 根目錄裏

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

如果你没有 pug 以及 stylus 的渲染器，请下载安装：
```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

**启用主题**

修改 Hexo 根目录下的 `_config.yml`，把主题改为 `butterfly`
```yaml
theme: butterfly
```

**主题的一些配置**

```yaml
# Menu 目錄
menu:
  Home: / || fas fa-home
  Archives: /archives/ || fas fa-archive
  Tags: /tags/ || fas fa-tags
  Categories: /categories/ || fas fa-folder-open
  List||fas fa-list:
    Music: /music/ || fas fa-music
    Movie: /movies/ || fas fa-video
  Link: /link/ || fas fa-link
  About: /about/ || fas fa-heart

social:
  fab fa-github: https://github.com/xxxx || Github || '#24292e'  

# Avatar (頭像)
avatar:
  img: https://avatars.githubusercontent.com/u/232521?s=400&v=4
  effect: false
```

> 具体关于主题的设置，直接看主题的文档吧，[butterfly](https://butterfly.js.org/posts/21cfbf15/)

然后再执行 hexo 的生成和部署，等待一会儿，再看效果。