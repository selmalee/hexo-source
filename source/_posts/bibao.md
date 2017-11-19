---
title: JavaScript学习笔记之闭包
date: 2016-08-06 20:57:42
tags: JavaScript
categories:
 - 学习笔记
---

## 作用域相关定义
&emsp;&emsp;在说闭包之前，我们首先说一下作用域。
&emsp;&emsp;JavaScript中有全局作用域，函数作用域。看下面的代码
<!--more-->
``` js
var a = 10;//全局作用域中定义变量
(function(){
	var b = 20;//函数作用域中定义变量
})();
console.log(a);//可以访问全局变量
console.log(b);//error, b is not defined
```
&emsp;&emsp;JavaScript是没有块级作用域的。看下面的代码

``` js
for(var item in {a:1,b:2}){
	console.log(item);
}
console.log(item); //没有块级作用域，可以访问item
```
&emsp;&emsp;这里顺带一提，ES6提出了块级作用域及新变量声明（let）。
&emsp;&emsp;JS使用var声明变量，以function来划分作用域，大括号“{}” 却限定不了var的作用域。用var声明的变量具有变量提升（declaration hoisting，即先试用后声明不报错）的效果。
&emsp;&emsp;ES6里增加了一个let，可以在{}， if， for里声明。用法同var，但作用域限定在块级，let声明的变量不存在变量提升。
## 什么是闭包
维基百科是这样说的：
> 在计算机科学中，闭包（也称词法闭包或函数闭包）是指一个函数或函数的引用，与一个引用环境绑定在一起。这个引用环境是一个存储该函数每个非局部变量（也叫自由变量）的表。
> 闭包，不同于一般的函数，它允许一个函数在立即词法作用域外调用时，仍可访问非本地变量。

&emsp;&emsp;JavaScript之所以有闭包，是因为它是一个第一类函数特性的语言，即可用把函数当做对象去传递作为返回值。
&emsp;&emsp;看下面的函数，对于这种函数，当调用函数outer()之后，局部变量localVal就可以被释放了。

``` js
function outer(){
	var localVal = 30;
	return localVal;
}
outer();//30
```
&emsp;&emsp;在JavaScript中，函数也是对象，并且函数也可以作为返回值也可以传参，函数里也可以套用别的函数。
&emsp;&emsp;对于下面的函数，调用函数outer()时，返回的是匿名函数，这个匿名函数里面依然可以访问外面的局部变量localVal。在调用outer()之后，调用func()，依然可以访问外部函数的局部变量localVal，localVal不会被释放。
&emsp;&emsp;这就是闭包。
``` js
function outer(){
	var localVal = 30;
	return function(){
		return localVal;
	}
}
var func = outer();
func();//30
```
## 闭包无处不在
&emsp;&emsp;如下面的函数，点击事件中依然可以访问外部函数的局部变量
``` js
!function(){
	var localData = "localData here";
	document.addEvenetListener("click",function(){
		console.log(localData);
	});
}
```
&emsp;&emsp;再如下面的异步请求，jquery的$.ajax方法，在整个函数调用结束之后，回调函数依然可以访问到url，localData这些局部变量
``` js
!function(){
	var localData = "localData here";
	var url = "http://www.qq.com/";
	$.ajax({
		url: url,
		success: function(){
			console.log(localData);
		}
	})
}
```
## 闭包的坑
&emsp;&emsp;看下面的函数，输出结果是什么？
``` js
document.body.innerHTML = "<div id='div1'>aaa</div>" + "<div id='div2'>bbb</div>" + "<div id='div3'>ccc</div>";
for(var i = 0; i < 4; i++){
	document.getElementById('div' + i)
		.addEventListener("click", function(){
			alert(i);//all are 4!
		}
	);
}
```
&emsp;&emsp;实际上，点击任何一个div输出结果都是4。
&emsp;&emsp;在点击某个div的时候，执行回调函数，这个时候函数才会动态地拿到i的值。这一切是在整个过程初始化之后的，在初始化之后i的值就已经是4了。
&emsp;&emsp;那怎么样才能达到想要的效果呢？看下面的代码
``` js
document.body.innerHTML = "<div id='div1'>aaa</div>" +
	"<div id='div2'>bbb</div>" + "<div id='div3'>ccc</div>";
for(var i = 0; i < 4; i++){
	!function(i){
		document.getElementById('div' + i)
			.addEventListener("click", function(){
				alert(i);//1,2,3
			}
		);
	}(i);
}
```
&emsp;&emsp;这里每一层循环的时候，用了立即执行的匿名函数把点击事件函数包装起来，并传参i，即1,2,3。那么每一次点击div，alert(i)里面的i就会取每一个闭包环境下的i，而这个i来自于每一次循环的i，所以点击每一个div的时候就会弹出相应的值了
## 闭包的作用
&emsp;&emsp;闭包可以用于封装一些变量，看下面的代码
``` js
(function){
	var _userId = 23492;
	var _typeId = 'item';
	var export = {};

	function converter(userId){
		return +userId;
	}

	export.getUserId = function(){
		return converter(_userId);
	}

	export.getTypeId = function(){
		return converter(_typeId);
	}
	window.export = export;
}

export.getUserId(); //23492
export.getTypeId(); //item

export._UserId; //undefined
export._TypeId; //undefined
export.converter; //undefined
```
&emsp;&emsp;这里定义了_userId等一些外部无法直接访问的局部变量，并通过window.export = export把对象export输出出去。在外部使用export的时候，只能通过export的一些方法来访问到函数的局部变量，而无法直接访问这些变量和方法。
&emsp;&emsp;这利用了闭包的特性，比如export.getUserId()函数，在整个匿名函数初始化之后，它依然能够访问到局部变量_userId。
&emsp;&emsp;同时，闭包也会带来一些问题，比如局部变量没有被释放掉造成空间浪费；内存泄露；性能消耗等。
## 参考
[JavaScript深入浅出](http://www.imooc.com/learn/277)

友情链接：[前端重点知识整理（JavaScript）三：闭包](http://blog.csdn.net/seminelee/article/details/52131659)
