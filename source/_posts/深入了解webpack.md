---
title: 深入了解webpack
date: 2018/05/01 22:04
tags: [前端,工具]
categories: 前端
comments: true
---
webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

从 webpack v4.0.0 开始，可以不用引入一个配置文件。然而，webpack 仍然还是高度可配置的。在开始前你需要先理解四个核心概念：
- 入口(entry)
- 输出(output)
- 加载器(loader)
- 插件(plugins)

# 入口
- 单入口
```js
const config = {
  entry: './path/to/my/entry/file.js'
};

module.exports = config;
```
entry 属性的单个入口语法，是下面的简写：
```js
const config = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
};
```