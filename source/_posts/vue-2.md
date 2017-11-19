---
title: Vue.js学习笔记二：vue-router2.0快速建构SPA项目
date: 2017-04-10 03:08:43
tags: [Vue, MV*框架]
categories:
 - 学习笔记
---

## 前言
时隔几个月，决定再写一篇关于Vue.js学习的文章（上一篇在[这里](https://seminelee.github.io/2016/08/18/vue-1/)）。
<!-- more -->
借此文来总结一下怎么样用vue2.0+webpack快速地搭建SPA的开发框架。
## 安装
Vue.js 提供一个官方命令行工具，可用于快速搭建大型单页应用。该工具提供开箱即用的构建工具配置，带来现代化的前端开发流程。只需几分钟即可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目：
``` bash
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目，中途会问你Install vue-router? (Y/n)填Y安装
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install
$ npm run dev
```
国内可以使用cnpm命令更节省时间。

简单说一下package.json里的部分配置
``` js
{
    // ...
    // npm run dev 开发环境，浏览器打开http://localhost:8080/
    // npm run build 生产环境
    "scripts": {
        "dev": "node build/dev-server.js",
        "build": "node build/build.js"
    },
    // ...
    "devDependencies": {
        // 打包编译
        "webpack": "^2.2.1",
        // 中间件，webpack 热重载(开发时实时刷新)
        "webpack-dev-middleware": "^1.10.0",
        "webpack-hot-middleware": "^2.16.1",
        // 配置分离，将配置定义在config/下，然后合并成最终的配置，以避免webpack.config.js臃肿
        "webpack-merge": "^2.6.1"
    },
    // ...
}
```
可以在config/index.js中修改端口号和更改打包文件的存放位置。
## 路由配置
新建好必要的文件，components/文件结构如下
``` bash
.
├── hello.vue
├── about.vue
├── notfound.vue
├── posts
│   ├── hotest.vue
│   └── latest.vue
└── posts.vue
```

在src/router/index.js中进行路由配置
``` js
import Vue from 'vue';
import Router from 'vue-router';
// 引入组件
import Hello from '@/components/hello';
import Posts from '@/components/posts.vue';
import Hotest from '@/components/posts/hotest';
import Latest from '@/components/posts/latest';
import About from '@/components/about';
import Notfound from '@/components/notfound';

// 挂载根实例
Vue.use(Router);

// 创建 router 实例
export default new Router({
    // 定义路由
    // 每个路由应该映射一个组件。
  routes: [
    {
        path: '/',
        name: 'Hello',
        component: Hello
    },
    {
        path: '/posts',
        name: 'posts',
        component: Posts,
        // 嵌套路由
        children: [
            { path: 'hotest', component: Hotest },
            { path: 'latest', component: Latest }
        ]
    },
    {
        path: '/about',
        name: 'about',
        component: About
    },
    {
        // 其余的不能匹配到的路由
        path: '*',
        component: Notfound
    }
  ]
})
```
App.vue
``` html
<template>
  <div id="app">
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <!-- "是否激活" 默认类名的依据是 inclusive match（全包含匹配）。按照这个规则，<router-link to="/"> 将会点亮各个路由！想要链接使用 "exact 匹配模式"，则使用 exact 属性 -->
    <router-link to="/" exact>首页</router-link>
    <router-link to="/posts">文章</router-link>
    <router-link to="/about">关于</router-link>
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>
```
posts.vue
``` html
<template>
    <div id="posts">
        <h1>文章</h1>
        <router-link to="/posts/hotest">最热文章</router-link>
        <router-link to="/posts/latest">最新文章</router-link>
        <!-- 路由匹配到的组件将渲染在这里 -->
        <router-view></router-view>
    </div>
</template>
```
其他组件就不一一细述了。

## 视图部分
现在打开浏览器可以看到是这个样子的。
首页 /
![首页](/static/2017/04/vue2-1.jpeg)
最热文章 /posts/hotest
![最热文章](/static/2017/04/vue2-2.jpeg)
### 引入iView
有点丑对不对，这里为大家推荐一套Vue.js 的高质量 UI 组件库，[iview](https://www.iviewui.com/docs/guide/install)。
这套组件库最近针对vue2.0也推出了iview2.0版本，且开源。我们这里用一下它的导航组件。
首先安装一下iview
``` bash
npm install iview --save-dev
```
引入iView
src/main.js
``` js
// ...
import 'iview/dist/styles/iview.css';
// ...
```
src/router/index.js
``` js
// ...
import iView from 'iview';
// ...
Vue.use(Router);
Vue.use(iView);

export default new Router({
  // ...
})
```
### 使用组件
现在可以尽情使用UI组件啦
App.vue
``` html
<template>
  <div id="app">
    <Menu mode="horizontal" theme="dark" active-name="1">
        <Menu-item name="1">
            <router-link to="/" exact><Icon type="ios-paper"></Icon>首页</router-link>
        </Menu-item>
        <Menu-item name="2">
            <router-link to="/posts"><Icon type="ios-people"></Icon>文章</router-link>
        </Menu-item>
        <Menu-item name="3">
            <router-link to="/about"><Icon type="settings"></Icon>关于</router-link>
        </Menu-item>
    </Menu>
    <router-view></router-view>
  </div>
</template>
```
posts.vue
``` html
<template>
    <div id="posts">
        <Menu mode="horizontal" theme="light" active-name="1">
            <Menu-item name="1">
                <router-link to="/posts/hotest">最热文章</router-link>
            </Menu-item>
            <Menu-item name="2">
                <router-link to="/posts/latest">最新文章</router-link>
            </Menu-item>
        </Menu>
        <router-view></router-view>
    </div>
</template>
```
基本上就好了，但是我们发现一个问题，刷新的时候导航不能实时显示当前路由所对应的菜单。优化如下
``` html
<template>
  <div id="app">
    <Menu mode="horizontal" theme="dark" :active-name="key">
        <Menu-item name="/">
          <router-link to="/" exact><Icon type="ios-paper"></Icon>首页</router-link>
        </Menu-item>
        <Menu-item name="/posts/hotest">
          <router-link to="/posts/hotest"><Icon type="ios-people"></Icon>文章</router-link>
        </Menu-item>
        <Menu-item name="/about">
            <router-link to="/about"><Icon type="settings"></Icon>关于</router-link>
        </Menu-item>
    </Menu>
    <router-view></router-view>
  </div>
</template>

<script>
import router from './router';
export default {
  name: 'app',
  mounted(){
    var rootKey = this.$route.path.split('/')[1];
    this.key = rootKey == '' ? '/' : rootKey;
  },
  data(){
    return {
      key: ''
    }
  }
}
</script>
<style>
/* ... */
li.ivu-menu-item a {
  display: inline-block;
  color: inherit;
  height: 60px;
}
</style>
```
posts.vue同理

## 常见报错
 - ERROR in ./src/view/BootPage.vue
template syntax error Component template should contain exactly one root element:...
实际意思：在vue-router2.0中，事件template的根节点只能有一个。
 - [Vue warn]: Failed to resolve directive: link
(found in anonymous component - use the "name" option for better debugging messages.)
实际意思：在vue-router1.0和vue-router2.0中有些指令和语法已经发生了变化。

## 代码地址

 - 体验地址：http://119.29.142.213/vue2demo/#/posts/hotest
 - 代码地址：https://github.com/seminelee/vue2-demo.git

## 题外话
多说崩了，换成了[来必力](https://www.drixn.com/2017/LiveReCommentsSystem/)，虽然是韩国的-_-但是看起来不会轻易垮。之前的评论全没有了= =深感抱歉
以后有机会想做一个自己网站用的评论系统> v <（不要乱立flag啊少女）
