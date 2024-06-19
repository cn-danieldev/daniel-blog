---
title: "使用 hugo 构建个人笔记网站"
date: 2024-06-19T00:17:37+08:00
# weight: 1
tags: ["Hugo"]
author: ["Daniel"]
draft: false
---

## 创建 github repo

命名随意，记住保持 repo 可见性为 `public`，方便后续 github page。并且不创建 `readme.md` 文件。

![alt-text](https://raw.githubusercontent.com/cn-danieldev/CDN/main/img/CleanShot%202024-06-19%20at%2010.32.54%402x.png)

> 后续 [repo_name] 指代的都是刚刚创建的 repo 名称。

创建好了之后将其 `clone` 下来。

## 安装 hugo

```shell
brwe install hugo
```

其他系统安装请参考 [官方安装教程](https://gohugo.io/installation/)。

## 添加主题

在刚刚 clone 的 repo 内部运行下述命令：

```shell
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)
```

更新 submodule：需 `cd` 至 `[repo_name]` 目录之后运行下述命令

```shell
git submodule update --remote --merge
```

## 修改主题样式

在 `[repo_name]` 目录下的 `content` 文件夹内创建 `archives.md` 和 `search.md`，根据 [官方 wiki](https://github.com/adityatelange/hugo-PaperMod/wiki/Features) 中的 `Archives Layout` 和 `Search Page` 内容进行填写。而后复制 [exampleSite](https://github.com/adityatelange/hugo-PaperMod/tree/exampleSite) 中的 `config.yml` 中的内容至 `[repo_name]` 目录下的 `hugo.yaml`。也可通过 [官方 wiki](https://github.com/adityatelange/hugo-PaperMod/wiki) 自行配置。下面为自用的配置文件：

```yaml
baseURL:  https://cn-danieldev.github.io
languageCode: en-us
title: Daniel'Log
description: Document my learning notes.  
name: "Daniel's Tech Blog"
theme: ["PaperMod"]
paginate: 7

enableInlineShortcodes: true
enableRobotsTXT: true
buildDrafts: false
buildFuture: false
buildExpired: false
enableEmoji: true
pygmentsUseClasses: true
mainsections: ["posts"]

minify:
  disableXML: true
  # minifyOutput: true

languages:
  en:
    languageName: "English"
    weight: 1
    taxonomies:
      category: categories
      tag: tags
      series: series
    menu:
      main:
        - name: Archive
          url: archives/
          weight: 5
        - name: Search
          url: search/
          weight: 10
        - name: Tags
          url: tags/
          weight: 10
        - name: WiKi
          url: https://github.com/adityatelange/hugo-PaperMod/wiki/

params:
  env: producton
  DateFormat: "January 2, 2006"
  defaultTheme: auto
  disableThemeToggle: false

  ShowShareButtons: false
  ShowReadingTime: true
  ShowPostNavLinks: true
  ShowBreadCrumbs: false
  ShowCodeCopyButtons: true
  ShowRssButtonInSectionTermList: true
  ShowAllPagesInArchive: true
  ShowPageNums: true
  ShowToc: true
  UseHugoToc: true
  displayFullLangName: false
  disableSpecial1stPost: false
  disableScrollToTop: false
  disableHLJS: true 
  comments: false
  hidemeta: false
  hideSummary: false

  homeInfoParams:
    Title: 👋 Welcome to Daniel'Log
    Content: Hi, this is Daniel. I'm documenting my learning notes in this blog.
  
  fuseOpts:
    isCaseSensitive: false
    shouldSort: true
    location: 0
    distance: 1000
    threshold: 0.4
    minMatchCharLength: 0
    limit: 10
    keys: ["title", "permalink", "summary", "content"]

outputs:
  home:
    - HTML
    - RSS
    - JSON

markup:
  goldmark:
    renderer:
      unsafe: true
  highlight:
    noClasses: false
```

完成上述步骤之后可通过下述命令进行测试：

```shell
hugo server
```


## 配置博客 Front matter

在 `[repo_name]` 目录下创建 `archetypes/post.md` 文件，以下为自用模版：

```md
---
title: ""
date: {{ .Date }}
# weight: 1
tags: [""]
author: [""]
draft: true
---
```

`draft: ture` 表示创建的博客为草稿，当要上传博客的时候需设定为 `false`。

## 创建博客和查看

配置好模版之后通过下述命令来创建博客：

```shell
hugo new --kind post posts/<name>
```

填写好博客内容之后可通过下述命令启动 `development server` 进行网站的查看：

```shell
hugo server -D
```

## 设置 Github Action

通过 [Hugo 官方教程](https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action) 进行设置即可。包含了自动将博客上传至 public 文件夹以及 deploy 博客网站。

## References

https://gohugo.io/getting-started/quick-start/

https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action

https://github.com/adityatelange/hugo-PaperMod/wiki/Installation

https://github.com/adityatelange/hugo-PaperMod/wiki/Features