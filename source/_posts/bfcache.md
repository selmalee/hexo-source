---
title: BFCache相关
date: 2016-08-29 16:47:00
tags: [bfcache,往返存储,pageshow,pagehide,persisted]
categories:
 - http相关
---
## 前言
一些浏览器中返回按钮是直接使用缓存的，不会执行任何js代码，例如, 在提交的时候将按钮设置为loading状态，如果在提交成功后没有对按钮进行处理，那么返回后按钮依然是loading状态，这种体验很差。
![example](http://119.29.142.213/static/201608/bfcacheex.png)
这是因为：
部分浏览器在后退时**不会**触发onload事件，這是HTML5世代浏览器新增的特性之一——Back-Forward Cache(简称bfcache)

## 什么是bfcache
我们熟悉的红本本《JavaScript高级程序设计》有提及bfcache：

 > bfcache，即back-forward cache，可称为“往返缓存”，可以在用户使用浏览器的“后退”和“前进”按钮时加快页面的转换速度。这个缓存不仅保存页面数据，还保存了DOM和JS的状态，实际上是将整个页面都保存在内存里。如果页面位于bfcache中，那么再次打开该页面就不会触发onload事件
 > 

<!--more-->
### pageshow事件
 这个事件在页面显示时触发，无论页面是否来自bfcache。在重新加载的页面中，pageshow会在load事件触发后触发；而对于bfcache中的页面，pageshow会在页面状态完全恢复的那一刻触发。

### pagehide事件
 该事件会在浏览器卸载页面的时候触发，而且是在unload事件之前触发。

### persisted属性
 pageshow事件和pagehide事件的event对象还包含一个名为persisted的布尔值属性。
 
  - 对于pageshow事件，如果页面是从bfcache中加载的，则这个属性的值为true；否则，这个属性的值为false。
  - 对于pagehide事件，如果页面在卸载之后被保存在bfcache中，则这个属性的值为true；否则，这个属性的值为false。

不同的浏览器在浏览器会在当前窗口“打开”历史纪录中的前一个页面的表现上并不统一，这和浏览器的实现以及页面本身的设置都有关系。

## 测试

### 测试代码
[线上示例](http://119.29.142.213/static/201608/bfcachetest.html)
js
``` js
function appendFunc(msg){
	var d = new Date();
	var li = document.createElement("li");
	li.innerHTML = d.toISOString().substr(14, 9) + " " + msg;
	document.querySelector("ul").appendChild(li);
}
window.addEventListener('load', function(){
	appendFunc('Load Event');
	document.querySelector("#changeColor").onclick = function(){
		document.querySelector("a").style.color = "red";
	}
})
window.addEventListener('pagehide', function( e ){
	appendFunc("Pagehide Event");
	appendFunc("pagehide persisted is :"  + e.persisted);
})

window.addEventListener('pageshow', function( e ){
	appendFunc("Pageshow Event");
	appendFunc("pageshow persisted is :"  + e.persisted);
})
```
DOM
``` html
<body>
<a href="http://www.sina.com.cn/">前往大新浪</a>
<input type="button" id="changeColor" value="变红" />
<ul></ul>
</body>
```
### 测试结果

 | browser | result  |
 | -------------| -----|
 | IE 11 | 新载入会触发load和pageshow事件，紅色未保留，没有bfcache |
 | Chrome 52.0.2743.116      | 新载入会触发load和pageshow事件，紅色未保留，没有bfcache |
 | Opera 39.0 | 新载入会触发load和pageshow事件，紅色未保留，没有bfcache |
 | Firefox 48.0.2  | 新载入会触发load和pageshow事件，点前往大新浪时触发pagehide事件，但不存入bfcache，紅色未保留 |
 | Safari(iPhone) iOS 9.3.5 | 新载入会触发load和pageshow事件，点前往大新浪时触发pagehide事件，存入bfcache ，紅色保留 |
 | UC | 新载入会触发load和pageshow事件，点前往大新浪时触发pagehide事件，存入bfcache ，紅色保留 |
 | qq浏览器 | 【ios 9.3.5】新载入会触发load和pageshow事件，回上页时不触发任何事件且红色被保留，有bfcache【安卓】新载入会触发load和pageshow事件，点前往大新浪时触发pagehide事件，显示没有存入bfcache ，但是红色保留，有bfcache但是不支持pagehide事件的persisted属性 （不同机型有不一样的结果）|
 | browser | 新载入会触发load和pageshow事件，回上页时不触发任何事件且红色被保留，有bfcache（存疑） |
 可以看到，Safari、Firefox、UC、qq浏览器、browser保留了红色，有往返缓存。
 回到上页时，
 
  - Safari、UC和qq浏览器都不会触发load；
  - IE、Firefox、Chrome和Opera会触发load；  
  - browser不触发任何事件。

## 解决方案
Firefox的[开发者文档](https://developer.mozilla.org/en-US/Firefox/Releases/1.5/Using_Firefox_1.5_caching)中提供了一些思路：

 - 页面监听了 unload 或者 beforeunload 事件;
 - 页面设置了 “cache-control: no-store”.
 - 网站使用 HTTPS 同时页面至少满足以下一个条件:
   - “Cache-Control: no-cache”
   - “Pragma: no-cache”
   -  设置请求头 “Expires: 0” 或者 “Expires” 的值为 “Date” 之前的值 (除非 “Cache-Control: max-age=” 也被设置了);
 - 页面在用户前进后退的时候还没有完全加载完或者它有正在进行的网络请求,比如 XMLHttpRequest;
 - 页面正在进行IndexedDB操作;
 - 顶层的页面包含有frame,并且这些frame由于这里列的任何一条原因而不能被缓存;
 - 页面在一个frame内，并且用户在这个frame内跳转到了一个新的网页,这里将被缓存的是新载入的网页

### JS监听pageshow事件阻止页面进入bfcache
一言不合放代码
``` js
window.addEventListener('pageshow', function( e ){
	if (e.persisted) {
		window.location.reload() 
	}
})
```
Safari、UC、qq浏览器测试通过。但是UC、qq浏览器会先闪过bfcache中的页面，因为pageshow是在load事件触发之后才触发的。browser依然会保留红色，我认为是因为browser回到上页时不触发任何事件。

### JS监听pagehide事件阻止页面进入bfcache
一言不合放代码
``` js
window.addEventListener('pagehide', function(e) {
	var $body = $(document.body);
	$body.children().remove();

    // 要等到回调函数完成，用户按返回才执行script标签的代码
    setTimeout(function() {
	    $body.append("<script type='text/javascript'>window.location.reload();<\/script>");
    });
});
```
Safari、UC、qq浏览器测试通过。browser依然会保留红色，我认为是因为browser回到上页时不触发任何事件。

### 给响应添加Cache-Control的header
代码示例如下：
在jsp模板的header部分加入如下的禁用缓存设置：
``` html
<head>
<%
  response.setHeader("Cache-Control","no-cache,no-store,must-revalidate");
  response.setHeader("Expires", "0");
  response.setHeader("Pragma","no-cache"); 
%>
</head>
```

### 安卓webview cache的解决方案
该方案的前提是浏览器在向server请求页面时，每次都用jsp重新生成html。需要页面本身有禁用缓存的配置。
``` html
<!-- 安卓webview 后退强制刷新解决方案 START -->
<jsp:useBean id="now" class="java.util.Date" />
<input type="hidden" id="SERVER_TIME" value="${now.getTime()}"/>
<script>
//每次webview重新打开H5首页，就把server time记录本地存储
var SERVER_TIME = document.getElementById("SERVER_TIME"); 
var REMOTE_VER = SERVER_TIME && SERVER_TIME.value;
if(REMOTE_VER){
    var LOCAL_VER = sessionStorage && sessionStorage.PAGEVERSION;
    if(LOCAL_VER && parseInt(LOCAL_VER) >= parseInt(REMOTE_VER)){
        //说明html是从本地缓存中读取的
        location.reload(true);
    }else{
        //说明html是从server端重新生成的，更新LOCAL_VER
        sessionStorage.PAGEVERSION = REMOTE_VER;
    }
}
</script>
<!-- 安卓webview 后退强制刷新解决方案 END -->
```
## 总结
1. PC浏览器，设置禁用页面缓存header即可实现后退刷新

2. 支持bfcache/page cache的移动端浏览器，JS监听pageshow/pagehide，在检测到后退时强制刷新

3. 在前2个方案都不work的情况下，可以在HTML中写入服务端页面生成版本号，与本地存储中的版本号对比判断是否发生了后退并使用缓存中的页面

## 参考
 - [Forcing mobile Safari to re-evaluate the cached page when user presses back button](http://stackoverflow.com/questions/24524248/forcing-mobile-safari-to-re-evaluate-the-cached-page-when-user-presses-back-butt/24524249#24524249)
 - [H5浏览器和webview后退刷新方案](http://hzxiaosheng.bitbucket.org/work/2015/09/23/refresh-webpage-on-history-back-for-mobile-browser-and-webview.html)
 - [瀏覽器在回上頁時不會觸發網頁onLoad事件](http://blog.darkthread.net/post-2012-08-02-trigger-onload-event-when-back-forward.aspx)
 - [BFcache是什么东西](http://jser.me/2012/09/22/BFcache%E6%98%AF%E4%BB%80%E4%B9%88%E4%B8%9C%E8%A5%BF.html)
 - [开发者文档](https://developer.mozilla.org/en-US/Firefox/Releases/1.5/Using_Firefox_1.5_caching)