---
title: JavaScript Promise API
date: 2016-08-28 11:01:57
tags: [Es6,Javascript]
categories:
 - 学习笔记
---

本文译自[https://davidwalsh.name/promises](https://davidwalsh.name/promises)
<!-- more -->
虽然同步代码更容易跟踪和调试，但是异步的性能和灵活性通常更好。为什么在你可以立刻发起多个请求，然后在每个都准备好了的时候处理它们时，要停下来等呢？Promises正在成为JavaScript世界的一个重要组成部分，有许多新的API正在与Promise理念来实施。下面让我们来看看promise及其API是如何使用的！

## **在自然环境下的Promises**
XMLHttpRequest API是异步的，但*不*使用Promises API。然而，现在有一些原生API使用promises：

 - [Battery API](https://davidwalsh.name/javascript-battery-api)
 - [fetch API](https://davidwalsh.name/fetch)(用来代替XHR)
 - ServiceWorker API (还没有该API文章)

Promises只会变得更加普遍，它很重要，所有的前端开发人员都应该熟悉它。另外值得一提的是，Node.js的是Promises的另一个平台（显然，Promises是一个核心的语言功能）。

*测试promises比你想象的更简单，因为`setTimeout`可以作为你的异步"任务"！*

## **Promise的基本用法**
`new Promise()`构造函数应该只用于传统异步任务，如`setTimeout`或`XMLHttpRequest`的用法。即，用关键字`new`创建一个新的Promise对象，该promise对象提供`resolve`和`reject`两个回调函数：
``` javascript
var p = new Promise(function(resolve, reject) {
	// Do an async task async task and then...

	if(/* good condition */) {
		resolve('Succes!';)
	}
	else {
		reject('Failure!');
	}
});

p.then(function() {
	/* do something with the result */
}).catch(function() {
	/* error :( */
})
```

这取决于开发者异步任务执行的结果，在回调函数中手动调用`resolve`或`reject`。一个典型的例子是，转换XMLHttpRequest成基于promise的任务：
``` javascript
// From Jake Archibald's Promises and Back:
// http://www.html5rocks.com/en/tutorials/es6/promises/#toc-promisifying-xmlhttprequest

function get(url) {
	// Return a new promise.
	return new Promise(function(resolve, reject) {
		// Do the usual XHR stuff
		var req = new XMLHttpRequest();
		req.open('GET', url);

		req.onload = function() {
			// This is called even on 404 etc
			// so check the status
			if (req.status == 200) {
				// Resolve the promise with the response text
				resolve(req.response);
			}
			else {
				// Otherwise reject with the status text
				// which will hopefully be meaningful error
				reject(Error(req.statusText));
			}
		};

		// Handle network errors
		req.onerror = function() {
			reject(Error("Network Error"));
		};

		// Make the request
		req.send();
	});
}

//Use it!
get('story.json').then(function(response) {
	console.log("success!", response);
}, fucntion(error) {
	console.log("Failed!", error);
});
```

有时候,你并不*需要*在promise内执行一个异步任务————但是，如果可能执行一个异步任务，返回一个promise对象将是最好的做法，这样，你只需要给定结果处理函数就可以了。在这种情况下，不需要使用关键字`new`你只需要简单地调用`Promise.resolve()`或`Promise.reject()`就可以了。 例如：
``` js
var userCache = {};

function getUserDetail(username) {
	// In both cases, cached or not, a promise will be returned

	if (userCache[username]) {
		// Return a promise without the "new" keyword
		return Promise.resolve(userCache[username]);
	}

	// Use the fetch API to get the information
	// fetch return a promise
	return fetch('users/' + username + '.json')
		.then(function(result) {
			userCache[username] = result;
			return result;
		})
		.catch(function() {
			throw new Error('Could not find user: ' + username);
		})
}
```

因为总是返回一个promise，你可以在它的返回值上随时使用`then`和`catch`方法！

##then##
所有promise实例有一个`then`方法，用来处理执行结果。第一个`then`方法回调接收`resolve()`调用的结果作为参数：
``` js
new Promise(function(resolve, reject) {
	// A mock async acton using setTimeout
	setTimeout(function() {
		resolve(10);
	}, 3000);
})
.then(function(result) {
	console.log(result);
})

// From the console
// 10
```

当promise的resolve触发，`then`回调函数被触发。你也可以使用链式的`then`回调方法：
``` js
new Promise(function(resolve, reject) {
	setTimeout(function() {
		resolve(10);
	}, 3000)
})
.then(function(num) {
	console.log('first then: ', num);
	return num * 2;
})
.then(function(num) {
	console.log('second then: ', num);
	return num * 2;
})
.then(function(num) {
	console.log('last then: ', num);
});

// From the console
// first then: 10
// second then: 20
//last then: 40
```

每个`then`接收之前的`then`返回值的结果。

如果一个promise的resolve已经触发，但之后`then`再次被调用，回调将立即触发。如果promise被拒绝，你被拒绝后调用`then`的话，回调不会被调用。

## **catch**
当`promise`被拒绝时，`catch`回调函数就会被执行：
``` js
new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() {
		reject('Done!');
	}, 3000);
})
.then(function(e) {
	console.log('done', e);
})
.catch(function(e) {
	console.log('catch: ', e);
})

// Frome the console:
// 'catch: Done!'
```

传入`reject`方法的参数由你决定。一般来说是传入一个`Error` 对象`：
``` js
reject(Error('Data could not be found'));
```

## Promise.all
想想JavaScript加载器的情形：有些时候你触发多个异步交互，但只希望在他们都完成后才作出响应————这就是`Promise.all`的用处所在。`Promise.all`方法接受一个promise数组，一旦他们的resolve都触发之后就执行一个回调函数：
``` js
Promise.all([promise1, promise2]).then(function(results) {
	// Both promises resolved
})
.catch(function(error) {
	// One or more promises was rejected
})
```
使用`Promise.all`的最佳示例是（通过`fetch`）同时发起多个 AJAX请求时:
``` js
var request1 = fetch('/user.json');
var request2 = fetch('./articles.json');

Promise.all([request1, request2]).then(function(results) {
	// Both promises done!
});
```
你可以结合API像`fetch`和Battery API，因为它们都返回promises：
``` js
Promise.all([fetch('./users.json'), navigator.getBattery()])
	.then(function(results) {
		// Both promises done!
	});
```
处理拒绝(rejection)当然是复杂的。如果任何promise被reject，`catch`将会被第一个拒绝(rejection)所触发。
``` js
var req1 = new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() {
		resolve('First!');
	}, 4000);
});
var req2 = new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() {
		reject('Second!');
	}, 3000);
});
Promise.all([req1, req2]).then(function(results) {
	console.log('Then: ', results);
}).catch(function(err) {
	console.log('Catch: ', err);
});

// From the console:
// Catch: Second!
```
随着越来越多API支持promises，`Promise.all`将会变得十分有用。

## Promise.race
`Promise.race`是一个有趣的函数————一旦数组中任何一个Promise被解决或者拒绝，`Promise.race`就会触发，而不会等待所有的promise对象被解决或者拒绝。
``` js
var req1 = new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() {
		resolve('First!');
	}, 4000);
});
var req2 = new Promise(function(resolve, reject) {
	// A mock async action using setTimeout
	setTimeout(function() {
		reject('Second!');
	}, 3000);
});
Promise.race([req1, req2]).then(function(results) {
	console.log('Then: ', results);
}).catch(function(one, two) {
	console.log('Catch: ', err);
});

// From the console:
// Then: Second!
```
用例可发起一个请求到主站资源和备用资源（以防主站和备用之一不可用）。

## 熟悉Promise
Promise在过去的几年里（或者在过去10年，如果你是一个Dojo Toolkit的用户）一直是热议的话题，他们已经从JavaScript框架的一部分变成一个JavaScript语言的主要部分。很有可能，你将看到大多数的新的JavaScript API都将基于promise实现...

......这是一个伟大的事情！开发人员能够避免回调地狱，而且异步交互可以像任何其他变量那样来传递。Promise还需要一段时间来普及，现在是学习他们的时候了！

 > **结束语**
 > 我觉得翻译带给我的好处是
 > 让我不停思考、认认真真地看第一手资料，而且可以慢慢看得懂其他前沿文章。
 > 想了解我更多的作品，请点击菜单[关于](https://seminelee.github.io/works/)
 >
