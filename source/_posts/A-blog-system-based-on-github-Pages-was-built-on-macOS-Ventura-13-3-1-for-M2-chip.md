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
sitemap: false
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

## sitemap

[hexo-generator-sitemap](https://github.com/hexojs/hexo-generator-sitemap) 是默认生成网站 sitemap 的插件。

**安装**

```bash
npm install hexo-generator-sitemap --save
```
支持版本：
* Hexo 4: 2.x
* Hexo 3: 1.x
* Hexo 2: 0.x

**配置**

在 `_config.yml`添加配置：
```yaml
sitemap:
  path: 
    - sitemap.xml
    - sitemap.txt
  template: ./sitemap_template.xml
  template_txt: ./sitemap_template.txt
  rel: false
  tags: true
  categories: true
```

* path - Sitemap 路径，默认sitemap.xml
* template - 生成 sitemap.xml 的模版
* template_txt - 生成 sitemap.txt 的模版
* rel - Add rel-sitemap to the site's header. (Default: false)
* tags - Add site's tags
* categories - Add site's categories

创建2个模版文件：

`sitemap_template.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in posts %}
  <url>
    <loc>{{ post.permalink | uriencode }}</loc>
    {% if post.updated %}
    <lastmod>{{ post.updated | formatDate }}</lastmod>
    {% elif post.date %}
    <lastmod>{{ post.date | formatDate }}</lastmod>
    {% endif %}
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>
  {% endfor %}

  <url>
    <loc>{{ config.url | uriencode }}</loc>
    <lastmod>{{ sNow | formatDate }}</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>

  {% for tag in tags %}
  <url>
    <loc>{{ tag.permalink | uriencode }}</loc>
    <lastmod>{{ sNow | formatDate }}</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.2</priority>
  </url>
  {% endfor %}

  {% for cat in categories %}
  <url>
    <loc>{{ cat.permalink | uriencode }}</loc>
    <lastmod>{{ sNow | formatDate }}</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.2</priority>
  </url>
  {% endfor %}
</urlset>
```

`sitemap_template.txt`
```txt
{% for post in posts %}{{ post.permalink | uriencode }}
{% endfor %}{{ config.url | uriencode }}
{% for tag in tags %}{{ tag.permalink | uriencode }}
{% endfor %}{% for cat in categories %}{{ cat.permalink | uriencode }}
{% endfor %}
```

排除生成 sitemap 配置：

Add sitemap: false to the post/page's front matter.
```markdown
---
title: {{ title }}
date: {{ date }}
tags:
categories:
keywords:
description:
top_img:
cover:
comments: false
sitemap: false
---
```

## 生成 rss

[hexo-generator-feed](https://github.com/hexojs/hexo-generator-feed) 是默认生成网站 rss 的插件。

**安装**

```bash
$ npm install hexo-generator-feed --save
```

支持的版本：
- Hexo 4+: 2.x
- Hexo 3: 1.x
- Hexo 2: 0.x

## Use

In the [front-matter](https://hexo.io/docs/front-matter.html) of your post, 
在模版文件里增加`description`, `intro` or `excerpt`配置：
```markdown
---
title: {{ title }}
date: {{ date }}
tags:
categories:
keywords:
description:
intro:
excerpt:
top_img:
cover:
comments: false
sitemap: false
---
``` 
> 不配置，摘要将默认为摘要或文章的前140个字符。

**配置**

在 `_config.yml`添加配置：

```yaml
feed:
  enable: true
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
  icon: icon.png
  autodiscovery: true
  template:
```
- **enable** - 是否开启. 默认开启。
- **type** - feed类型. `atom` or `rss2`. Specify `['atom', 'rss2']` to output both types. (Default: `atom`)
  * Example:
  ``` yaml
  feed:
    # Generate atom feed
    type: atom

    # Generate both atom and rss2 feeds
    type:
      - atom
      - rss2
    path:
      - atom.xml
      - rss2.xml
  ```
- **path** - Feed path. When both types are specified, path must follow the order of type value. (Default: atom.xml/rss2.xml)
- **limit** - Maximum number of posts in the feed (Use `0` or `false` to show all posts)
- **hub** - URL of the PubSubHubbub hubs (Leave it empty if you don't use it)
- **content** - (optional) set to 'true' to include the contents of the entire post in the feed.
- **content_limit** - (optional) Default length of post content used in summary. Only used, if **content** setting is false and no custom post description present.
- **content_limit_delim** - (optional) If **content_limit** is used to shorten post contents, only cut at the last occurrence of this delimiter before reaching the character limit. Not used by default.
- **order_by** - Feed order-by. (Default: -date)
- **icon** - (optional) Custom feed icon. Defaults to a gravatar of email specified in the main config.
- **autodiscovery** - Add feed [autodiscovery](https://www.rssboard.org/rss-autodiscovery). (Default: `true`)
  * Many themes already offer this feature, so you may also need to adjust the theme's config if you wish to disable it.
- **template** - Custom template path(s). This file will be used to generate feed xml file, see the default templates: [atom.xml](atom.xml) and [rss2.xml](rss2.xml).
  * It is possible to specify just one custom template, even when this plugin is configured to output both feed types,
  ``` yaml
  # (Optional) Exclude custom template from being copied into public/ folder
  # Alternatively, you could also prepend an underscore to its filename, e.g. _custom.xml
  # https://hexo.io/docs/configuration#Include-Exclude-Files-or-Folders
  exclude:
    - 'custom.xml'
  feed:
    type:
      - atom
      - rss2
    template:
      - ./source/custom.xml
    # atom will be generated using custom.xml
    # rss2 will be generated using the default template instead
  ```

然后再执行 hexo 的生成和部署，等待一会儿，再看效果。