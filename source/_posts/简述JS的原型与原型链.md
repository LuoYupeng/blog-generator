---
title: 简述JS的原型与原型链
date: 2018-12-20 22:40:08
tags:
---

#### 什么是原型

ECMAScript中规定全局对象叫做 global，但是浏览器把 window 作为全局对象（浏览器先存在的）

window 就是一个哈希表，有很多属性。window 的属性就是全局变量。

这些全局变量分为两种：

1. 一种是 ECMAScript 规定的
   - global.parseInt
   - global.parseFloat
   - global.Number
   - global.String
   - global.Boolean
   - global.Object
2. 一种是浏览器自己加的属性
   - window.alert
   - window.prompt
   - window.comfirm
   - window.console.log
   - window.console.dir
   - window.document
   - window.document.createElement
   - window.document.getElementById

> 由于JavaScript的对象创建不存在拷贝，对象的原型实际上也是一个对象，它和对象本身是完全独立的两个对象。原型是为了共享多个对象之间的一些共有特性（属性或方法），JavaScript中的对象，都有一个内置属性`[Prototype]`，指向这个对象的原型对象。

每个创建对象除了自身属性`hasOwnProperty`外，都会有`__proto__`指向公用`key`属性。

#### 什么是原型链

```js
var 对象 = new 函数()
对象.__proto__ === 对象的构造函数.prototype
```

> 在JavaScript 中，每个对象都有一个指向它的原型（prototype）对象的内部链接。这个原型对象又有自己的原型，直到某个对象的原型为 null 为止（也就是不再有原型指向），组成这条链的最后一环。这种一级一级的链结构就称为**原型链**。

例如：

```js
var s1 = new String('123')
s1.__proto__ === String.prototype // true
String.prototype.__proto__ === Object.prototype //true
Object.prototype.__proto__ === null //true
```

```
![WX20181221-034313@2x.png](https://i.loli.net/2018/12/21/5c1bf11d60b60.png)
```

因为Number，String，Object是Function的实例，所以:

```js
var num = new Number()
num.__proto__ === Number.prototype
Number.__proto__ === Function.prototype //true 因为 Number 是 Function 的实例

var obj = new Object()
obj.__proto__ === Object.prototype
Object.__proto__ === Function.prototype //true 因为 Object 是 Function 的实例

var fn = new Function()
fn.__proto__ === Function.prototype
Function.__proto__ === Function.prototype //true 因为 Function 是 Function 的实例！

Function.prototype.__proto__ === Object.prototype //true
Object.prototype.__proto__ === null //true
```





参考文献：

[白话原型和原型链](https://juejin.im/post/599d69fc6fb9a0248f4a7b31)

[浅谈JS原型和原型链](https://zhuanlan.zhihu.com/p/23026595)

[深刻理解JavaScript基于原型的面向对象](https://www.iteye.com/topic/1126635)

