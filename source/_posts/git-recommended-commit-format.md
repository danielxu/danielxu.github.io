---
title: git 推荐 commit 格式
comments: false
sitemap: false
date: 2023-08-10 09:48:28
tags:
	- git
	- commit
categories: git
keywords: 'git,commit'
description: 基于 Angular 的 Commit message 写法规范，推荐使用此提交消息格式，方便查看与构建docs。
top_img:
cover:
---
{% note blue 'fas fa-bullhorn' %}

一般来说，一个比较好的 commit message 应该清晰明了，说明本次提交的目的。目前社区有多种 Commit message 的写法规范，不过目前使用最广泛的写法是 [Angular 规范](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.greljkmo14y0)，比较合理和系统化，并且有配套的工具。

{% endnote %}

# 为什么要格式化

格式化的Commit message，有几个好处。

* 提供更多的历史信息，方便快速浏览。

比如，下面的命令显示上次发布后的变动，每个commit占据一行。你只看行首，就知道某次 commit 的目的。
```bash
$ git log <last tag> HEAD --pretty=format:%s
```

* 可以过滤某些commit（比如文档改动），便于快速查找信息。

比如，下面的命令仅仅显示本次发布新增加的功能。
```bash
$ git log <last release> HEAD --grep feature
```

* 可以直接从commit生成Change log。

Change Log 是发布新版本时，用来说明与上一个版本差异的文档。
![Change Log](./img/change_log.png)


# 推荐提交格式
为了便于提交的消息在github及各种工具上阅读，提交消息的每一行不得超过100个字符！！！

提交的消息包含：头部，正文，页脚，使用空行进行分割
```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>(closes:[#TASK_NAME](https://))
```

## Revert
如果当前 commit 用于撤销以前的 commit，则必须以 `revert:` 开头，后面跟着被撤销 Commit 的 Header。

Body部分的格式是固定的，必须写成  `This reverts commit <hash>.`，其中的 `hash` 是被撤销 commit 的 SHA 标识符。
  * 如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。
  * 如果两者在不同的发布，那么当前 commit，会出现在 Change log 的 Reverts 小标题下面。

例如：
```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```
## Message header
消息头只有一行，简介的说明。包含的 格式：`<type>(<scope>): <subject>`

### Allowed `<type>`

> 必填

* feat - 新功能,
* fix - 修补bug,
* docs - 只文档变更,
* style - 不影响代码含义的更改(空白、格式化、缺少分号等),
* refactor - 重构,即不是新增功能，也不是修改bug的代码变动,
* perf - 改进性能的代码更改,
* test - 添加缺失的测试或更正现有的测试,
* build - 影响构建系统或外部依赖的更改 (example scopes: gulp, broccoli, npm),
* ci - 对 CI 配置文件和脚本的更改(example scopes: Travis, Circle, BrowserStack, SauceLabs)"),
* chore -  其他不修改src或测试文件的更改, 构建过程或辅助工具的变动
  
### Allowed `<scope>`

用于说明 commit 影响的范围，可以是指定提交更改位置的任何内容。
比如数据层、控制层、视图层、$location， $browser， $compile， $rootScope, ngHref, ngClick, ngView等。

> 如果没有更合适的作用域，可以使用 * 。

### `<subject>` text
是 commit 目的的简短描述，不超过50个字符。
> 必添。

* 以动词开头，使用第一人称现在时，比如 change，而不是 changed 或 changes
* 第一个字母小写
* 结尾不加句号（.）

## Message body
> 可选

详细说明，回答如下问题:
* 为什么这种改变是必要的?
* 它如何解决这个问题?
* 有什么副作用吗?

> 有两个注意点。
> 1. 使用第一人称现在时，比如使用 change 而不是 changed 或 changes。
> 2. 应该说明代码变动的动机，以及与以前行为的对比。

参考：
* [Writing Git commit messages](https://365git.tumblr.com/post/3308646748/writing-git-commit-messages)
* [A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)

例如：
```
更详细的解释性文字。大约72个字符左右。

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
```

## Message footer
Footer 部分只用于两种情况。
> 如下2种情况，必写

### BREAKING CHANGE
不兼容变动。如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。

例如：
```
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```
### Referencing issues
格式：`Closes [<#task_name>](http://)`，已关闭的bug应该在页脚的单独一行中列出，并以 “Closes” 关键字为前缀，如下所示:

```
closes [#123](https://xxx.123)
```

可以关闭多个
```
closes #123, #245, #992
```

# 范例

```
feat($browser): onUrlChange event (popstate/hashchange/polling)

Added new event to $browser:
- forward popstate event if available
- forward hashchange event if popstate not available
- do polling when neither popstate nor hashchange available

Breaks $browser.onHashChange, which was removed (use onUrlChange instead)
```

```
fix($compile): couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

Closes #392
Breaks foo.bar api, foo.baz should be used instead
```

```
feat(directive): ng:disabled, ng:checked, ng:multiple, ng:readonly, ng:selected

New directives for proper binding these attributes in older browsers (IE).
Added coresponding description, live examples and e2e tests.

Closes #351
```

```
style($location): add couple of missing semi colons
```

```
docs(guide): updated fixed docs from Google Docs

Couple of typos fixed:
- indentation
- batchLogbatchLog -> batchLog
- start periodic checking
- missing brace
```

```
feat($compile): simplify isolate scope bindings

Changed the isolate scope binding options to:
  - @attr - attribute binding (including interpolation)
  - =model - by-directional model binding
  - &expr - expression execution binding

This change simplifies the terminology as well as
number of choices available to the developer. It
also supports local name aliasing from the parent.

BREAKING CHANGE: isolate scope bindings definition has changed and
the inject option for the directive controller injection was removed.

To migrate the code follow the example below:

Before:

scope: {
  myAttr: 'attribute',
  myBind: 'bind',
  myExpression: 'expression',
  myEval: 'evaluate',
  myAccessor: 'accessor'
}

After:

scope: {
  myAttr: '@',
  myBind: '@',
  myExpression: '&',
  // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
  myAccessor: '=' // in directive's template change myAccessor() to myAccessor
}
```

# Commitizen
[Commitizen](https://github.com/commitizen/cz-cli) 是一个撰写合格 Commit message 的工具。详细操作可以去项目主页查看，以下只是简单解释。

安装命令如下。
```bash
$ npm install -g commitizen
```
然后，在项目目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式。
```bash
$ npm init --yes  # 生成 package.json 文件
$ commitizen init cz-conventional-changelog --save --save-exact
```
以后，凡是用到`git commit`命令，一律改为使用`git cz`。这时，就会出现选项，用来生成符合格式的 Commit message。

# validate-commit-msg
[validate-commit-msg](https://github.com/kentcdodds/validate-commit-msg) 用于检查 Node 项目的 Commit message 是否符合格式。

使用一下命令安装：
```bash
npm install --save-dev validate-commit-msg
```

可以指定 .vcmrc 配置项目，必须是有效 JSON 数据文件。以下是默认内容：
```json
{
  "types": ["feat", "fix", "docs", "style", "refactor", "perf", "test", "build", "ci", "chore", "revert"],
  "scope": {
    "required": false,
    "allowed": ["*"],
    "validate": false,
    "multiple": false
  },
  "warnOnFail": false,
  "maxSubjectLength": 100,
  "subjectPattern": ".+",
  "subjectPatternErrorMsg": "subject does not match subject pattern!",
  "helpMessage": "",
  "autoFix": false
}
```

可选配置，在package.json里面制定。

```json
  "config": {
    "validate-commit-msg": {
      /* your config here */
    }
  }
```
> .vcmrc 优先级最高，如果不存在，则使用 package.json 里的配置

这为您提供了一个二进制文件，您可以将其用作 githook 来验证提交消息，[husky](http://npm.im/husky).你会想要把这一部分加入到 `commit-msg` 脚本中，例如，当使用 husky 时，在 package.json 中的 [npm scripts](https://docs.npmjs.com/misc/scripts)中添加 `"commitmsg": "validate-commit-msg"`。

然后，每次`git commit`的时候，这个脚本就会自动检查 Commit message 是否合格。如果不合格，就会报错。

```bash
$ git add -A 
$ git commit -m "edit markdown" 
INVALID COMMIT MSG: does not match "<type>(<scope>): <subject>" ! was: edit markdown
```
# 生成 Change log
如果你的所有 Commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成（[例1](https://github.com/conventional-changelog/conventional-changelog/blob/master/packages/conventional-changelog/CHANGELOG.md)，[例2](https://github.com/karma-runner/karma/blob/master/CHANGELOG.md)，[例3](https://github.com/btford/grunt-conventional-changelog/blob/master/CHANGELOG.md)）。

生成的文档包括以下三个部分。
* New features
* Bug fixes
* Breaking changes.

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

[conventional-changelog](https://github.com/ajoslin/conventional-changelog) 就是生成 Change log 的工具，运行下面的命令即可。
```bash
$ npm install -g conventional-changelog
$ cd my-project
$ conventional-changelog -p angular -i CHANGELOG.md -w
```

上面命令不会覆盖以前的 Change log，只会在CHANGELOG.md的头部加上自从上次发布以来的变动。

如果你想生成所有发布的 Change log，要改为运行下面的命令。
```bash
$ conventional-changelog -p angular -i CHANGELOG.md -w -r 0
```

为了方便使用，可以将其写入package.json的scripts字段。
```json
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -w -r 0"
  }
}
```
以后，直接运行下面的命令即可。
```bash
$ npm run changelog
```
