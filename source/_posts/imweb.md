---
title: imweb2017会议个人总结
date: 2017-09-17 22:07:53
tags: imweb
categories:
 - 学习笔记
---
## PWA与AMP-移动Web的现在与未来
比起native app，移动web的优势有这些：
 1. 跨平台 reach更多的用户
 2. 即时更新 更灵活 有错误也可以补救
 3. native app要为不同平台设计与开发 移动web工作量小一点

如今在移动互联网中，手机流量远胜于Desktop，而且比起移动Web更集中于Apps，并且是Top3的App。如何尝试扭转这个趋势呢？

<!-- more -->

### AMP(Accelerated Mobile Pages)
AMP是一个Github上开源的项目，它的目的是加速移动网络的网页加载从而提升体验。
它主要包括这三个方面：
 - AMP runtime。它新定义了一套HTML标签，如amp-img, amp-video。使用这些tag可以确保加载的顺滑。同时amp也负责管理资源何时加载，避免不必要的流量。
 - AMP validator 是一个校验器。为了确保网页能够达到所要求的极速，AMP对网页里能使用的东西做了严格的限制。比如不允许使用自己的JS，不允许外部加载CSS。校验器是为了帮助你检查你的网页是否达到了这些要求。
 - Google AMP cache 提供了第三方的缓存。这样从Google的搜索结果里进入AMP网页就可以做到预加载甚至预渲染，从来带来极速体验，就像前面的范例视频那样。当然，任何第三方都可以提供自己的缓存。

同时，由于这些限制，AMP目前主要针对静态内容的页面，如交互少，对JS依赖也少的新闻网站、博客等。

### PWA(Progressive Web Apps)
PWA是Google 提出的用前沿的 Web 技术为网页提供 App 般使用体验的一系列方案。
一个PWA应用首先是一个网页。可以通过Web技术编写出一个网页应用，随后添加上 App Manifest 和 Service Worker来实现PWA的安装和离线等功能。
为了让 PWA 应用被添加到主屏幕, 使用 manifest.json 定义应用的名称, 图标等等信息。然后在 HTML 文件当中引入配置。
Service Worker主要是解决离线应用的问题。Service Worker 可以使你的应用先访问本地缓存资源，所以在离线状态时，在没有通过网络接收到更多的数据前，仍可以提供基本的功能。它的生命周期大致如下：
 1. register 注册
 2. install 安装
 3. activated 抓取资源写入缓存，激活
 4. idle 空闲
 5. active 监听activate事件，遍历cache，如果有更新，清除旧缓存，更新资源
 6. terminate 销毁

详细API可在[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Service_Worker_API/Using_Service_Workers)上查看。详细例子可看github上google给的[sample](https://github.com/GoogleChrome/samples/tree/gh-pages/service-worker/basic)

## TypeScript和BuckleScript
### TypeScript-高效可扩展的JavaScript
TypeScript是JavaScript的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。
### BuckleScript
BuckleScript是把已经存在将近30年的语言 OCaml 编译成可读的 JS编译成可读的JavaScript的很好的编译器。它具有闪电一样的编译速度，且比TypeScript有更好的类型安全（能够保证一旦编译通过就不会有类型错误）。

## 从HTTP到Socket，深入浅出现代前端抓包技术
代理本地抓包只能适用于双网卡和同一wifi环境下的情况，因此尝试在node.js接入层抓包

## 框架工具
 - Tree-Shaking 删除冗余，包括dead code、无用的模块。webpack2 也引入了tree-shaking 的能力

## 面向未来的直播技术－WebRTC
目前的直播技术
 - HLS协议 移动兼容性高，但延迟高
 - Flv.js b站开源的视频播放器，原理是通过media source Extensions实现，在PC端兼容性高，但是在移动端兼容性差
 - WebRTC(Web Real-Time Communication) 兼容主流的PC端浏览器，延迟低,但移动端兼容性不好。

## XSS魅影危机之从编码说起
 - 服务层 ngnix、apache等等防御模块
 - 浏览器层 [X-XSS-Protection](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-XSS-Protection)、[CSP](https://www.w3.org/TR/CSP/)
 - 代码层 encoder、filter
 - 底线 HttpOnly(为Cookie提供了一个新属性，用以阻止客户端脚本访问Cookie)、域分离

### encoder
 - 过滤参数，转义"、<、>等，进行html属性编码、js编码
 - encodeurl url编码

### filter
需要特殊字符的情况，如富文本编辑器等
黑白名单过滤原则

## WebAssembly
Javascript 一开始就是解释性语言，后来引入了JIT技术（即时编译），但由于弱类型等编译速度较慢。
WebAssembly是用来编写高性能的、浏览器无关的 Web 组件的一种字节码规范。它通过静态类型变量实现性能提升，并且允许任何语言编译到它制定的AST tree。更适合用于写模块，承接各种复杂的计算，如图像处理、3D运算、语音识别、视音频编码解码这种工作


## 参考
 - [如何评价AMP (Accelerated Mobile Pages) Project？](https://www.zhihu.com/question/36380698/answer/105498519)
 - [PWA 入门: 写个非常简单的 PWA 页面](https://zhuanlan.zhihu.com/p/25459319)
