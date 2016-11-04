---
title: ajax与跨域
date: 2016-09-20 21:00:02
tags: [Ajax,跨域,JavaScript,jQuery]
categories:
 - http相关
---

最近想整理一下ajax与跨域方面的知识。
首先，我来复习一下ajax的写法，大神们自行绕过。

## ajax

AJAX = Asynchronous JavaScript and XML（异步的 JavaScript 和 XML）。它是在不重新加载整个页面的情况下，与服务器交换数据并更新部分网页的艺术。
<!-- more -->
### 基本写法

``` js
var xhr = createXHR();
xhr.onreadystatechange = function(){
	if (xhr.readyState == 4) {
		if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304) {
			alert(xhr.responseText);
		} else {
			alert("request was unsuccesful: " + xhr.status);
		}
	}
};
xhr.open("post", "postexample.php", true);
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
var form = document.getElementById("user-info");
xhr.send(serialize(form));

// xhr.open("get", url);
// xhr.send(null);
```

### jquery ajax 常用写法

``` js
$.ajax({
	method: "POST",
	url: "example.php",
	// method: "GET",
	// url: "example.php?number=" + $("#keyword").val(),
	dataType: "json",
	data: {
		name:$("#name").val(),
		number:$("#number").val(),
		sex:$("#sex").val(),
		job:$("#job").val()
	}
}).done(function(msg) {
	alert("Data saved:" + msg);
}).fail(function(jqXHR, textStatus) {
	console.log("Request failed: "+ textStatus);
})
```

### Promise对象实现Ajax

``` js
var getJSON = function(url) {
	var promise = new Promise(function(resolve, reject) {
		var xhr = new XMLHttpRequest();
		xhr.open("get", url);
		xhr.onreadystatechange = handler;
		xhr.responseType = "json";
		xhr.setRequestHeader("Accept", "application/json");
		xhr.send();

		function handler() {
			if(xhr.onreadyState !== 4) {
				return;
			}
			if(xhr.status === 200) {
				resolve(this.response);
			} else {
				reject(new Error(this.statusText));
			}
		}
	});

	return promise;
};

getJSON("/example.json").then(function(json) {
	console.log('Contenes: ' + json); // success
}), function(error) {
	console.log("出错了", error); // failure
}
```

## 跨域
### 同源策略
什么是跨域？什么是同源策略(same-origin policy)？
浏览器的同源策略，限制了来自不同源的"document"或脚本，是浏览器为安全性考虑实施的安全策略。URL由协议、域名、端口和路径组成，如果两个URL的协议、域名和端口相同，则表示他们同源。如果协议、域名和端口其中有一个不同，就被当作是跨域。

### 同源策略的限制
 - Cookie、LoalStorage无法读取
 - DOM无法获得
 - Ajax请求不能发送

如何打破这些限制呢？
 - document.domain：两个网页一级域名相同、二级域名不同则可设置相同的document.domain共享cookie。
 - window.name：通过在子窗口（iframe窗口、window.open打开的窗口）将信息写入window.name，跳回父窗口，父窗口读取window.name，则可获取对方的DOM。
 - window.postMessage：HTML5的跨文档通信 API（Cross-document messaging），允许跨窗口通信，包括读写localDtorage

以上不作细说，以下细说ajax跨域的方法：

## Ajax跨域的常用方法
### JSONP
JSON with padding（填充式JSON/参数式JSON），即被包含在函数调用中的JSON。JSONP由回调函数和数据组成。
它的基本思想是，由于`<img>`、`<script>`都能不受限制地从其他域加载资源，网页通过添加一个`<script>`元素，向服务器请求JSON数据，这种做法不受同源政策限制。服务器收到请求后，将数据放在一个指定名字的回调函数里传回来。
JS代码如下：
``` js
function addScriptTag(src) {
	var script = document.createElement("script");
	script.setAttribute("type","text/javascript");
	script.src = src;
	document.body.appendChild(script);
}
function foo(data) {
	console.log('success:' + data.msg);
}
window.onload = function() {
	addScriptTag("http://119.29.142.213/static/201609/demo.php?callback=foo");
}
```
也可以用jQuery实现：
``` js
$.ajax({
	type: "GET",
	//jsonp方式只支持GET请求
	url: "http://119.29.142.213/static/201609/demo.php",
	dataType: "jsonp",
	jsonp: "callback",
	success: function(data){
		if(data.success){
			console.log(data.msg);
		} else {
			console.log("出现错误：" + data.msg);
		}
	},
	error: function(jqXHR){
		alert("发生错误:" + jqXHR.status)
	}
})
```
php代码如下：
``` php
<?php
	$jsonp = $_GET["callback"];
	$result = $jsonp . '({"success":true,"msg":"msg from server"})';
	echo $result;
?>
```

### CORS(Cross Origin Resource Share)
CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。整个CORS通信过程，都是浏览器自动完成，不需要用户参与。只需要服务器实现CORS接口就可以了。
我尝试用php实现CORS接口，加入如下代码：
``` php
$origin=isset($_SERVER['HTTP_ORIGIN'])?$_SERVER['HTTP_ORIGIN']:'';
/************** 获取客户端的Origin域名 **************/
header('Access-Control-Allow-Origin:'.$origin);
```
过程是这样的：urlA上的页面获取urlB上的资源，浏览器会检查服务器B的HTTP头(HEAD请求)，如果Access-Control-Allow-Origin中有urlA，或者是通配符*，浏览器就会允许跨域。
跨域请求成功的请求头如下。浏览器发现这次跨源AJAX请求是简单请求，就自动在头信息之中，添加一个Origin字段。因为是我本地文件发出的，所以这里是file://。
```
Accept:*/*
Accept-Encoding:gzip, deflate, sdch
Accept-Language:zh-CN,zh;q=0.8
Cache-Control:max-age=0
Connection:keep-alive
Host:119.29.142.213
Origin:file://
User-Agent:Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36
```
回应头如下，Access-Control-Allow-Origin的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求。
```
Access-Control-Allow-Origin:file://
Connection:keep-alive
Content-Length:12
Content-Type:text/html
Date:Wed, 21 Sep 2016 13:53:41 GMT
Server:nginx/1.0.15
Vary:Accept-Encoding
X-Powered-By:PHP/5.2.17p1
```
非简单请求（非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。）这里不再细述，[阮一峰的跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)有更详细的解释。

### WebSocket
WebSocket使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。这种协议专门为快速传输小数据设计，适合用于移动端。
JS代码
``` js
var socket = new WebSocket("ws://127.0.0.1:8888/");
var message = {
	time: new Date(),
	text: "Hello from my file"
}
socket.onopen = function(e) {
	socket.send(JSON.stringify(message));
}
socket.onmessage = function(event) {
	console.log(event.data);
}
```
用NodeJS搭建本地服务器
``` js
var http = require('http');
var ws = require('ws');

var app = http
	.createServer(function(req,res){
		res.writeHead(200, {'Content-Type': 'text/plain'});
		res.write('Hello WebSocket from server');
		res.end();
	})
	.listen(8888);
var WebSocketServer = require('ws').Server,
    wss = new WebSocketServer( { server : app } );

console.log('Server running at http://127.0.0.1:8888');
```
请求头如下
```
GET ws://127.0.0.1:8888/ HTTP/1.1
Host: 127.0.0.1:8888
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Origin: file://
Sec-WebSocket-Version: 13
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.116 Safari/537.36
```
回应头如下
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: wUpcqQA/Kzv80w5vM90HFf3WN10=
Sec-WebSocket-Extensions: permessage-deflate
```

### 区别
 - JSONP只支持GET请求，但优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。
 - CORS支持所有类型的HTTP请求。
 - WebSocket的特别之处是它是双向通信的，

## XSS与CRSF攻击
在这里顺带一提XSS与CSRF攻击

### XSS
XSS（Cross-site scripting，跨站脚本），是注入攻击的一种。即攻击者的输入没有经过严格的控制进入了数据库，最终显示给来访的用户，导致可以在来访用户的浏览器里执行这些代码。例如发布评论，提交含有 JavaScript 的内容文本。这时服务器端如果没有过滤或转义掉这些脚本，作为内容发布到了页面上，其他用户访问这个页面的时候就会运行这些脚本。
可能是无聊恶意的`<script>alert(哈哈哈你关不掉我的~)</script>`，也可能是恶意修改站点原数据，甚至通过跨域请求直接加载恶意站点以窃取cookie和登录信息等，例如，建立一个收集信息的恶意网站，然后在评论中写入获取cookie、对恶意网站发起请求的代码，从而把包含了用户的帐号和其他隐私的信息发送到收集服务器上。
一些防御措施：
 - 对输入进行处理、过滤，把`<`转换成`&lt;`等等
CSRF（Cross-site request forgery，跨站请求伪造）。
 - 设置HttpOnly属性，防止劫取Cookie，js只能读到带有HttpOnly属性的Cookie

### CSRF
XSS 是实现 CSRF 的诸多途径中的一条。CRSF（Cross-site request forgery，跨站请求伪造），是伪造请求，冒充用户在站内的正常操作。即用户登录受信任网站A，并在本地生成Cookie。然后在不登出A的情况下，访问危险网站B。
例如，银行网站A，它以GET请求来完成银行转账的操作，如：`http://www.mybank.com/Transfer.php?toBankId=11&money=1000`。危险网站B利用`<img>`能不受限制地从其他域加载资源，写入代码`<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>`。这样，你的浏览器会带上你的银行网站A的Cookie发出Get请求，结果银行网站服务器收到请求后，认为这是一个更新资源操作（转账操作），所以就立刻进行转账操作。如果网站用的是POST请求，还可以利用XSS注入JS代码发送POST请求等等。
一些防御措施：
 - 判断 referer：在HTTP头中有一个字段叫Referer，它记录了该HTTP请求的来源地址。网站可以对每一个请求验证其Referer值，如果Referer是其他网站的话就拒绝该请求。但是也有侵入者手动改变了referer值的可能。
 - 在请求地址中添加token并验证：系统开发者可以在HTTP请求中以参数的形式加入一个随机产生的token，并在服务器端建立一个拦截器来验证这个token，如果请求中没有token或者token内容不正确，则认为可能是CSRF攻击而拒绝该请求。如果放到地址不够安全，也可以把token放到HTTP头中自定义的属性并验证。
 
详细解释请看参考文章[XSS 和 CSRF 攻击](http://www.cnblogs.com/imwtr/p/4763457.html)。

## 参考
 - [浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
 - [跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
 - [XSS 和 CSRF 攻击](http://www.cnblogs.com/imwtr/p/4763457.html)
 - [总结 XSS 与 CSRF 两种跨站攻击](https://blog.tonyseek.com/post/introduce-to-xss-and-csrf/)
 - 《JavaScript 高级程序设计》