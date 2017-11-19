---
title: Vue.js学习笔记一：入门
date: 2016-08-18 19:05:16
tags: [Vue, MV*框架]
categories:
 - 学习笔记
---

## 前言
　　vue是法语中视图的意思，Vue.js是一个轻巧、高性能、可组件化的MVVM库。
　　`MV*`可能大家都经常听说，我们先来理解一下`MV*`的概念。
<!--more-->
### 出现的背景
　　MVC开始是存在于桌面程序中的，但由于后端的mvc框架的v层越来越重，后端的MVC思想就搬移到了前端。随着前端代码越来越重，能力越来越大，重前端的系统越来越多地涌现出来。前端为主的MV*时代中，前端在MVC的结构指导下分为model(模型), view(视图)， controller(控制器)三部分。而controller慢慢演化为presenter和viewmodel。MVC, MVP, MVVM框架不断涌现。
### MVC, MVP, MVVM
- MVC(model-view-controller)，如backbone, angular(较高版本是mvvm, 也许说它是MVW更准确)。
　　View: 与页面上元素直接相关的部分，包括html，CSS和一部分直接控制页面元素的JS。它可以从Model中得到数据，并将其显示到页面上。
　　Model: 与后端的沟通、AJAX请求以及对数据的处理。Model本身不知道谁是View，谁是Controller。它只提供一些方法供View和Controller调用，并且将变更通知给它的观察者。
　　Controller: Model和View的粘合剂。Controller将View方面的请求转发给合适的Model，作为Model的观察者，获取Model的变更,在必要时更新View。

- MVP(model-view-presenter)使用此模型的框架不多，现在几乎倒向MVVM。MVP 模式将 Controller 改名为 Presenter，同时改变了通信方向。
　　Presenter，与Controller一样，接收View的命令，对Model进行操作；与Controller不同的是Presenter会反作用于View，Model的变更通知首先被Presenter获得，然后Presenter再去更新View。
- MVVM(model-view-viewmodel)如Vue.js。将Controller改为ViewModel。它与MVP的区别是，它采用双向绑定（data-binding）：View的变动，自动反映在ViewModel。
　　下图来自[对MVC、MVP、MVVM的理解](http://blog.csdn.net/napolunyishi/article/details/22722345)，清晰地说明它们之间的区别
![mvc,mvp和mvvm](/static/2016/08/mvc.png)

## Vue.js起步
### 简单介绍一下
 - 尤雨溪老师写的一个用于创建 web交互界面的库，是一个精简的MVVM，和其他库相比是一个小而美的库
 - 官方文档很清晰，比 Angular 简单易学
 - 采用双向绑定，用解耦的组件组合你的应用程序
 ![vue.js概况](/static/2016/08/vue1.png)

　　下图是[vue.js官网](http://vuejs.org/)上的双向绑定的小例子。首先是VM -> V，VM中的data里的message变化，会自动反映在V中的p标签里大括号内的message；V -> VM，V中input里输入的值，会自动反映到VM中的message值。所以，你在输入框中输入的文字会被实时显示成上方的文字。
![vue.js官网例子](/static/2016/08/vuedemo.png)
### 安装
　　安装步骤[vue.js官网](https://vuejs.org.cn/guide/installation.html)上介绍得十分清楚。这里我推荐先安装[淘宝镜像](https://npm.taobao.org/)，再进行安装Vue.js官方命令行工具，这样会更快。
``` bash
# 全局安装 cnpm淘宝镜像
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
# 全局安装 vue-cli
$ cnpm install -g vue-cli
# 创建一个基于 "webpack" 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ cnpm install
$ npm run dev
```

## 一些常用的指令
### v-model
　　用法：在表单控件(`<input>`、`<select>`、`<textarea>`)上创建双向绑定。
　　例子：输入框中初始化文字是"hello vue.js"，而你在输入框中输入的文字会被实时显示成上方的文字。
``` html
<div id="app">
	<p>{{newItem}}</p>
	<input v-model="newItem"/>
</div>
```
``` js
new Vue({
    el:'#app',
    data: {
        newItem:'hello vue.js.'
    }
});
```
### v-for
　　用法：基于源数据将元素或模板块重复数次，简单来说就是列表渲染。如果之前学过Angular会觉得很相似。
　　例子：会显示列表，列表中有"No.1"、"No.2"。
``` html
<ul>
  <li v-for="item in items">
    {{item.label}}
  </li>
</ul>
```
``` js
new Vue({
    el:'#app',
    data: {
        newItem:'hello vue.js.',
        items: [
        	{label: "No.1",isFinished: false},
        	{label: "No.2",isFinished: true}
        ]
    }
});
```
### v-on
　　用法：绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符（如.stop、.prevent等）也可以省略。
　　例子：`<li>`标签上绑定了点击事件，每次点击会改变item的isFinished属性。v-on:click可以缩写成@click。
``` html
<ul>
  <li v-for="item in items" v-on:click="toggleFinish(item)">
    {{item.label}}
  </li>
</ul>
```
``` js
new Vue({
    methods: {
      toggleFinish: function(item) {
        item.isFinished = !item.isFinished
      }
    }
});
```
### v-bind
　　用法：动态地绑定一个或多个 attribute，或一个组件prop到表达式。在绑定 class 或 style 时，支持其它类型的值，如数组或对象；在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。
　　例子：如果`<li>`的数据item.isFinished是true，`<li>`的class就是finished，文字颜色就会变成#ccc。v-bind:class可以缩写成:class。
``` html
<ul>
  <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
    {{item.label}}
  </li>
</ul>
```
``` css
.finished {
  color: #ccc;
}
```
### 父向子组件传参
　　例子：App.vue为父，引入componetA组件之后，则可以在template中使用<component-a>标签（注意驼峰写法要改成componet-a写法，因为html对大小写不敏感，componenta与componentA对于它来说是一样的，不好区分，所以使用小写-小写这种写法）。而子组件componetA中，声明props参数'msgfromfa'之后，就可以收到父向子组件传的参数了。例子中将msgfromfa显示在`<p>`标签中。
App.vue中
``` html
<component-a msgfromfa="(Just Say U Love Me)"></component-a>
```
``` js
import componentA from './components/componentA'
export default {
	new Vue({
	    components: {
	      componentA
	    }
	})
}
```
componentA.vue中
``` html
<p>{{ msgfromfa }}</p>
```
``` js
export default {
	props: ['msgfromfa']
}
```
### 父向子组件传参（.$broadcast）
　　用法：vm.$broadcast( event, […args] )广播事件，通知给当前实例的全部后代。因为后代有多个枝杈，事件将沿着各“路径”通知。
　　例子：父组件App.vue中`<input>`绑定了键盘事件，回车触发addNew方法，广播事件"onAddnew"，并传参this.items。子组件componentA中，注册"onAddnew"事件，打印收到的参数items。
App.vue中
``` html
<div id="app">
	<input v-model="newItem" @keyup.enter="addNew"/>
</div>
```
``` js
import componentA from './components/componentA'
export default {
	new Vue({
		methods: {
		  addNew: function() {
		    this.$broadcast('onAddnew', this.items)
		  }
		}
	})
}
```
componentA.vue中
``` js
import componentA from './components/componentA'
export default {
	events: {
	    'onAddnew': function(items){
	      console.log(items)
	    }
	}
}
```
### 子组件向父传参（.$emit）
　　用法：vm.$emit( event, […args] )，触发当前实例上的事件。附加参数都会传给监听器回调。
　　例子：App.vue中component-a绑定了自定义事件"child-say"。子组件componentA中，单击按钮后触发"child-say"事件，并传参msg给父组件。父组件中listenToMyBoy方法把msg赋值给childWords，显示在`<p>`标签中。
App.vue中
``` html
<p>Do you like me? {{childWords}}</p>
<component-a msgfromfa="(Just Say U Love Me)" v-on:child-say="listenToMyBoy"></component-a>
```
``` js
import componentA from './components/componentA'
export default {
	new Vue({
		data: function () {
		    return {
		      childWords: ""
		    }
		},
		components: {
		  componentA
		},
	    methods: {
		    listenToMyBoy: function (msg){
		      this.childWords = msg
		    }
		}
	})
}
```
componentA.vue中
``` html
<button v-on:click="onClickMe">like!</button>
```
``` js
import componentA from './components/componentA'
export default {
	data: function () {
	    return {
	      msg: 'I like you!'
	    }
	},
    methods: {
      onClickMe: function(){
        this.$emit('child-say',this.msg);
      }
    }
}
```
### 子组件向父传参（.$dispatch）
　　用法：vm.$dispatch( event, […args] )，派发事件，首先在实例上触发它，然后沿着父链向上冒泡在触发一个监听器后停止。
　　例子：App.vue中events中注册"child-say"事件。子组件componentA中，单击按钮后触发"child-say"事件，并传参msg给父组件。父组件中"child-say"方法把msg赋值给childWords，显示在`<p>`标签中。
App.vue中
``` html
<p>Do you like me? {{childWords}}</p>
<component-a msgfromfa="(Just Say U Love Me)"></component-a>
```
``` js
import componentA from './components/componentA'
export default {
	new Vue({
		events: {
		    'child-say' : function(msg){
		      this.childWords = msg
		    }
		}
	})
}
```
componentA.vue中
``` html
<button v-on:click="onClickMe">like!</button>
```
``` js
import componentA from './components/componentA'
export default {
	data: function () {
	    return {
	      msg: 'I like you!'
	    }
	},
    methods: {
      onClickMe: function(){
        this.$dispatch('child-say',this.msg);
      }
    }
}
```
　　这里只提及了一些指令，更多功能建议在官网上刷一遍[API文档](http://vuejs.org.cn/api/#vm-emit)。
## 做一个todolist
　　用以上的指令写一个简单的demo。实现添加事情，删除事情，点击事情表示事情已完成，点赞等功能。代码思路源自慕课网教程，我作了一些修改。
　　详细代码如下：
### src/App.vue
``` html
<template>
  <div id="app">
    <h1 v-text="title"></h1>
    <input v-model="newItem" @keyup.enter="addNew"/>
    <ul>
      <li v-for="item in items" v-bind:class="{finished: item.isFinished}" v-on:click="toggleFinish(item)">
        ❤ {{item.label}}
        <span v-on:click="deleteThis(item)">delete</span>
        <hr/>
      </li>
    </ul>

    <p>Do you like me? {{childWords}}</p>
    <component-a msgfromfa="(Just Say U Love Me)"></component-a>
  </div>
</template>
<script>
import Store from './store'
import componentA from './components/componentA'
//相当于module.export
export default {
  /*function data(){
    return...
  }*/
  /*相当于var vue = new vue({data: function(){}})*/
  data () {
    return {
      title: 'TODO LIST',
      items: Store.fetch() || [],
      newItem: '',
      childWords: ''
    }
  },
  components: {
    componentA
  },
  watch: {
    items: {
      handler: function(items){
        Store.save(items)
      },
      deep: true
    }
  },
  events: {
    'child-say' : function(msg){
      this.childWords = msg
    }
  },
  methods: {
    toggleFinish: function(item) {
      item.isFinished = !item.isFinished
    },
    addNew: function() {
      this.items.push({
        label: this.newItem,
        isFinished: false
      })
      this.newItem = ''
      this.$broadcast('onAddnew', this.items)
    },
    listenToMyBoy: function (msg){
      this.childWords = msg
    },
    deleteThis: function(item) {
      this.items.splice(this.items.indexOf(item), 1)
      Store.save(this.items)
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
}
html {
  height: 100%;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  width: 100%;
}

#app {
  color: #2c3e50;
  font-family: Source Sans Pro, Helvetica, sans-serif;
  text-align: center;
  width: 60%;
}

#app a {
  color: #42b983;
  text-decoration: none;
}
#app h1:nth-child(1) {
  line-height: 3;
  position: absolute;
  top: 10%;
}
#app input {
  width: 60%;
  line-height: 24px;
  font-size: 24px;
  position: absolute;
  top: 25%; left: 20%;
}

ul {
  position: absolute;
  top: 35%;
  text-align: left;
  width: 60%;
  height: 45%;
  overflow: auto;
}

ul li {
  list-style: none;
  line-height: 2;
  font-size: 24px;
}
span {
  font-size: 16px;
  color: #fff;
  padding: 2px 5px;
  text-align: right;
  background-color: red;
  border-radius: 5px;
}
.logo {
  width: 100px;
  height: 100px
}
.finished {
  color: #ccc;
}
hr {
  ;border-top:1px dashed #ccc;
}
p {
  text-align: left;
  position: absolute;
  bottom: 10%;
}
</style>
```

### src/components/componentA.vue

``` html
<template>
  <div class="hello">
    <p>{{ msgfromfa }}</p>
    <button v-on:click="onClickMe">like!</button>
  </div>
</template>
<script>
export default {
  data: function () {
    return {
      msg: 'I like you!'
    }
  },
  props: ['msgfromfa'],
  events: {
    'onAddnew': function(items){
      console.log(items)
    }
  },
  methods: {
    onClickMe: function(){
      this.$dispatch('child-say',this.msg);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: #42b983;
}
button {
  position: absolute;
  bottom: 10%;
  right: 20%;
  width: 100px;
  background-color: green;
  font-size: 16px;
  color: #fff;
  padding: 2px 5px;
  border: none;
}
.hello p {
  bottom: 5%;
  font-size: 12px;
  color: green;
}
</style>
```

### src/store.js

``` js
const STORAGE_KEY = 'todos-vuejs'//es6语法 const定义一个常量
export default {
	fetch () {//es6语法 相当于 fetch:function(){}
		return JSON.parse(window.localStorage.getItem(STORAGE_KEY) || '[]')
	},
	save (items) {
		window.localStorage.setItem(STORAGE_KEY, JSON.stringify(items))
	}
}
```
　　完成后如下图：
![ToDoList](/static/2016/08/todolist.png)


## 参考
- [vuejs入门基础（慕课网视频）](http://www.imooc.com/view/694)
- [Vue.js 快速入门](https://segmentfault.com/a/1190000003968020)
- [谈谈MVC模式](http://www.ruanyifeng.com/blog/2007/11/mvc.html)
- [大前端技术分享](http://zakwu.me/node/assets/ppts/tpls/bigFe.html)
- [对MVC、MVP、MVVM的理解](http://blog.csdn.net/napolunyishi/article/details/22722345)
