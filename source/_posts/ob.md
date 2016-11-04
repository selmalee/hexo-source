---
title: JavaScript学习笔记之对象及继承
date: 2016-08-07 00:37:00
tags: [JavaScript,原型链,继承]
categories: 
 - JavaScript
---

## 对象属性
　　ES5中有两种属性，数据属性和访问器属性。
　　数据属性包括[[writable]]（能否修改属性的值）、[[value]]等等；
　　访问器属性包括[[Configurable]]（能否通过delete删除属性、能否修改属性的特性）、[[Enumerable]]（能否通过for-in循环返回属性）、[[Get]]、[Set]]
　　要修改属性则使用Object.defineProperty()，这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。其中描述符对象的属性必须是：configurable、enumerable、writable和value。如下面的代码

``` js
var person = {};
Object.defineProperty(person, "name", {
	configurable: false,
	value: "Nicholas"
});

alert(person.name); //"Nicholas"
delete person.name;
alert(person.name); //"Nicholas"
```
<!--more-->
　　也可以使用Object.defineProperties()定义多个属性

``` js
var book = {};
Object.defineProperties(book,{
	_year: {
		value: 2004
	},
	edition: {
		value: 1
	},
	year: {
		get: function(){
			return this._year;
		},
		set: function(newValue){
			if(newValue > 2004) {
				this._year = newValue;
				this.edition += newValue - 2004;
			}
		}
	}
});
```

## 创建对象

### 工厂模式

``` js
function createPerson(name, age, job){
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
		alert(this.name);
	};
	return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor");
```
　　抽象了出案件具体对象的过程，每次调用函数会返回一个对象

### 构造函数模式

``` js
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
		alert(this.name);
	};
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor");

alert(person1.constructor == Person);//true
alert(person1 instanceof Object);//true
alert(person1 instanceof Person);//true
```
　　构造函数没有显性地创建对象；直接将属性和方法赋给了this对象；没有return语句。
　　创建实例的步骤是这样的：创建一个新对象；将构造函数的作用于赋给新对象（this指向这个新对象）；执行构造函数中的代码；返回新对象。
　　我们也可以将构造函数当做函数：

``` js
Person("Greg", 27, "Doctor");
window.sayName(); //"Greg"

//在另一个对象的作用域中调用
var o = new Object();
Person.call(o, "Kristen", 25, "Nurse");
o.sayName();
```
　　而构造函数的主要问题就是，每个方法都要在每个实例上重新创建一遍，不同实例上的同名函数是不相等的。

### 原型对象

　　原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。即不必在构造函数中定义对象实例的信息，而是可以将这些信息直接添加到原型对象中。
看下面的代码

``` js
function Person(){}
Person.prototype = {
	name : "Nicholas",
	age : 29,
	job : "Software Engineer",
	sayName : function(){
		alert(this.name);
	}
};

var Person1 = new Person();
var Person2 = new Person();

alert(person1.hasOwnProperty("name"));//false
alert("name" in Person1);//true

person1.name = "Greg";
alert(person1.name);//"Greg"  ——来自实例
alert(hasPrototypeProperty(person, "name"));//false
alert(person1.hasOwnProperty("name"));//true
alert("name" in Person1);//true

alert(person2.name);//"Nicholas"  ——来自原型
alert(person2.hasOwnProperty("name"));//false
alert("name" in Person1);//true

delete person1.name;
alert(person1.name);//"Nicholas"  ——来自原型
alert(person1.hasOwnProperty("name"));//false
alert("name" in Person1);//true
```
　　当然原型对象也有缺点：所有实例在默认情况下都取得相同的属性值，即共享性。这个问题尤其对于包含引用类型值的属性来说更为突出。
　　所以，组合使用构造函数模式与原型模式是更好的选择。即在构造函数中定义实例属性，而所有实例共享的属性在原型中定义。

## 什么是原型链
　　JavaScript主要是依靠原型链实现继承。
　　什么是原型链？我们来看看构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。看下面的代码
``` js
function SuperType(){
	this.property = true;
}
SuperType.prototype.getSuperValue = function(){
	return this.property;
}
function SubType(){
	this.subProperty = false;
}
//继承了SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function(){
	return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue()); //true
```
　　instance.getSuperValue()调用时会经历三个步骤：
- 搜索实例；
- 搜索SubType.prototype；
- 搜索SuperType.prototype，最后一步才找到方法。

　　而所有函数的默认原型都是Object的实例，所以完整的原型链如下图：
![来自《JavaScript高级程序设计（第3版）》](http://img.blog.csdn.net/20160807001109843)
　　即`instance.__proto__`（instance的原型）指向SubType.prototype，`SubType.prototype.__proto__`指向SuperType.prototype，而`SuperType.prototype.__proto__`指向Object.prototype，最后`Object.prototype.__proto__`指向null。
　　果然对象搞到头还是空啊！（开个玩笑==）
## 原型链的问题
　　原型链可以用来实现继承，但它也存在一些问题。
　　最主要的问题就是在创建原型对象中提及的，包含引用类型值的原型属性会被所有实例共享。
## 借用构造函数
　　鉴于上面的问题，我们可以通过使用apply()和call()在新创建的对象上执行构造函数。
``` js
function SuperType(){
	this.colors = ["red", "blue", "green"];
}
function SubType(){
	SuperType.call(this);//继承SuperType
}

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"

var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green"
```
　　实际上，我们在新创建的SubType实例的环境下调用了SuperType构造函数。这样，SubType的每个实例就都会有自己的colors属性的副本了。

　　此外，还有组合继承，原型式继承，寄生式继承等。脑袋内存有限，这里不再作探究。
本文主要参考《JavaScript高级程序设计（第3版）》。

## 参考
《JavaScript高级程序设计（第3版）》

### 友情链接
[前端重点知识整理（JavaScript）四：对象及继承](http://blog.csdn.net/SemineLee/article/details/52140233)