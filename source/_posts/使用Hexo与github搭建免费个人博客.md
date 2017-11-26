---
title: 使用Hexo与github搭建免费个人博客
date: 2017/11/26 18:11
tags: ["前端","博客"]
categories: 前端
comments: true
---
## 一、介绍

　　Hexo是一款快速、简洁且高效的博客框架，支持markdown等语法，另外具有强大的插件系统和众多的插件，高度可定制。

## 二、安装

1. 最新版本的[GIT](http://nodejs.org/)与[Node.js](http://nodejs.org/)
2. 安装Hexo脚手架
```
npm install -g hexo-cli
```

## 三、建站
　　请执行如下命令创建一个工程目录。
```
hexo init <folder>
cd <folder>
npm install
```

　　你会得到如下目录：
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   └── _posts
└── themes
```

**_config.yml**

　　网站的配置信息，您可以在此配置大部分的参数。

**package.json**

　　应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。

**scaffolds**

　　模版文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

　　Hexo的模板是指在新建的markdown文件中默认填充的内容。例如，如果您修改scaffold/post.md中的Front-matter内容，那么每次新建一篇文章时都会包含这个修改。

**source**

　　资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

**themes**

　　主题文件夹。Hexo会根据主题来生成静态页面。

## 四、配置
　　我们可以在_config.yml中修改大部分的配置，其中有些配置是必须要修改的。

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://halzhan.cn # 填入博客地址
root: / # 如果博客地址在子目录，请将子路径填入
permalink: :year/:month/:day/:title/
permalink_defaults:
```

　　一个比较重要的配置是github地址（下方是我的配置）：
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:HalZhan/halzhan.github.io.git # Git地址（http/https/ssh皆可）
  branch: master # 所要发布到的分支名称
```

　　另外一个是站点的基础配置：
```
# Site
title: Hal Zhan # 站点标题
subtitle: 不拘一格、Geek # 站点副标题
description: 
author: Hal Zhan(风起云涌Hal) # 作者
language: zh-Hans # 编码（汉字的话使用这个既可以了）
timezone:
```

　　其他的配置详见[官方文档-配置](https://hexo.io/zh-cn/docs/configuration.html)。

　　完成上述配置后，我们可以使用`hexo server`本地伺服站点。