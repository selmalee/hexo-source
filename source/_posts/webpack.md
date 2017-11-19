---
title: 理解webpack中间件
date: 2017-07-23 17:09:45
tags: [Webpack, 自动化构建工具]
categories:
 - 学习笔记
---
## webpack-dev-server
webpack-dev-server是一个小型的Node.js Express服务器，它使用webpack-dev-middleware来服务于webpack的包。在实际操作中，它将在localhost:8080(或其他端口)启动一个express静态资源web服务器，并且以监听模式自动运行webpack，并通过socket.io服务实时监听资源的变化并自动刷新页面（热更新）。
<!-- more -->
## webpack-dev-middleware
webpack-dev-middleware是一个中间件。
什么是中间件？中间件(middleware)是一个函数，它可以访问请求对象、响应对象和web应用中处于请求－响应循环流程中的中间件。它的功能包括：执行任何代码、修改请求和响应对象、终结请求－响应循环和调用堆栈中的下一个中间件。
具体来说，过程是这样的：webpack本身只负责打包编译的功能，webpack-dev-server是协助我们开发的服务器，它通过webpack-dev-middleware作为中间件，从请求和响应的过程中取得webpack编译好的资料，进行热更新。
## webpack-hot-middleware
webpack-hot-middleware是给webpack-dev-middleware用的，就是让我们的服务器加上热更新的功能。它会订阅监听开发服务器，当更新或异动发生的时候就透过webpack的HMR API来更新。
总的来说，webpack-dev-middleware ＋ webpack-hot-middleware ＝ 有热更新功能的webpack开发服务器


## 参考
 - [手把手深入理解 webpack dev middleware 原理與相關 plugins](https://segmentfault.com/a/1190000005614604?_ea=868190)
 - [Setting Up Webpack Dev Middleware in Express](http://madole.github.io/blog/2015/08/26/setting-up-webpack-dev-middleware-in-your-express-application/)
 - [Express官网－使用中间件](http://www.expressjs.com.cn/guide/using-middleware.html)
 - [Webpack中文指南－开发环境](https://zhaoda.gitbooks.io/webpack/content/development.html)
