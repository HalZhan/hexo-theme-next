---
title: 使用hexo与github搭建免费个人博客
date: 2017/11/26 18:11
tags: ["前端","博客"]
categories: 前端
comments: true
---
## 一、介绍

　　Hexo是一款快速、简洁且高效的博客框架，支持markdown等语法，另外具有强大的插件系统和众多的插件，高度可定制。

　　github是目前最为流行的源码托管网站之一，我们在创建github账户时，会被分配一个静态的虚拟服务器（只能伺服静态资源，并且每月有流量限制，不过也绰绰有余）。访问`<用户名>.github.io`这个地址，实际上就是我们的github个人主页。

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

　　完成上述配置后，我们可以使用`hexo server`本地伺服站点。如果需要在控制台中显示更多debug信息，可以加上`--debug`选项。

## 五、发布
　　我们需要在github上预先创建一个空的仓库，请注意这个仓库的名称必须叫做：`<用户名>.github.io`，例如本例中的仓库名称叫做：`halzhan.github.io`。

　　我们需要将本地目录与远程仓库联系起来。回到根目录下，执行如下命令：
```
git init
...
git add .
git commit -m "init project"
git remote add origin "git@github.com:HalZhan/halzhan.github.io.git"
git push origin master
```

**注：推荐再创建一个分支（本例中创建了一个hexo分支），用于保存我们的站点源码，而master分支用于存放最终发布的站点文件。**

```
git checkout -b hexo
```

　　安装一个模块：
```
npm install --save hexo-deployer-git
```

　　再回到根目录下_config.yml文件中，对发布选项做一些配置：
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:HalZhan/halzhan.github.io.git # Git地址（http/https/ssh皆可）
  branch: master # 所要发布到的分支名称
```

　　发布的话请使用如下命令：
```
hexo clean && hexo g && hexo d
```

　　再次访问`<用户名>.github.io`，如果一切顺利应该可以看到刚刚发布的站点。

## 六、主题
　　github上有许多非常优秀的hexo主题，这里推荐一个比较流行的主题，[next](https://github.com/iissnan/hexo-theme-next)，首先需要将这个主题下载到我们的项目里：
```
git clone https://github.com/iissnan/hexo-theme-next.git themes/next
```

　　返回到项目根目录下的`_config.yml`文件中，搜索字段`theme`，将其值改为`next`，然后可以执行命令`hexo server --debug`本地查看一下效果。

　　next主题有许多配置选项，我们可以在`themes/next`目录下更改`_config.yml`里选项的值。

　　例如显示社交链接：
```
social:
  GitHub: https://github.com/halzhan || github # github地址
  E-Mail: mailto:halzhan163@163.com || envelope # 邮件
  Weibo: https://weibo.com/halzhan || weibo # 微博主页
  LinkedIn: https://www.linkedin.com/in/hal-zhan-a0041587/ || linkedin # 领英主页
  #Google: https://plus.google.com/yourname || google
  #Twitter: https://twitter.com/yourname || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  #Instagram: https://instagram.com/yourname || instagram
  #Skype: skype:yourname?call|chat || skype
```

　　显示查看更多按钮（首页文章内容截取，这样就不会在首页展示全部内容）
```
# Automatically Excerpt. Not recommend.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: true
  length: 150
```

　　启用Gemini风格的next主题：
```
# Schemes
# scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini
```

　　每种主题都会有许多配置选项，可以自行尝试修改并查看效果。

## 七、常见问题
1. 主题同步

　　这个问题是比较头疼的，因为目前hexo主题都是通过git clone的方式克隆到本地来的，然而在提交站点源码到远程仓库时，themes文件夹下的主题文件是不会提交到仓库里的，因此我们如果更改了主题文件下的一些设置，这部分设置由于没有提交会被丢失，这带来了极大的不便。为此，我们决定采用官方推荐的fork+subtree的方式来管理主题。

- 删除themes目录下的next文件夹，并将修改push到github
```
git add .
git commit -m "delete next"
git push
```

- 进入next的github主页，fork之

- 待fork完成后绑定next主题为子项目
```
git remote add -f next git@github.com:HalZhan/hexo-theme-next.git
git subtree add --prefix=themes/next next master --squash
```

- 更新子项目
```
git fetch next master
git subtree pull --prefix=themes/next next master --squash
```

- 从子目录push到远程仓库
```
git subtree push --prefix=themes/next next master
```

- 提交并发布本地修改到远程仓库
```
git add .
git commit -m "next theme"
git push
```

　　现在再去远程仓库看themes/next/就有内容了，并且跟子项目的远程仓库可以保持更新，在主项目中修改也可以push到子项目的远程。这样就不必担心主题配置丢失了。