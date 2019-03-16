---
title: JS基础：Function
date: 2019-03-03 16:20:04
tags:
---

### 函数的声明方法

1、具名函数：

~~~js
function x(输入1，输入2){
   return undefined}
~~~

2、匿名函数：(需要给一个变量)

~~~js
var x
x = function(){}
~~~

3、把具名函数赋值给变量：

```	js
var x = function y(){
  console.log(y) //会报错： y is not defined
}
```

4、使用`window.Function`函数对象

```js
// new Function('x','y','return x + y')
var n = 1
f = new Function('x','y','return x +' +n+ '+y')
f(1,2) //4
```

5、箭头函数(也属于匿名函数)

```js
// (x,y) => {return x + y}
(x,y) => x + y //只能只有一句语句时适合
n => n*n     	 //只有一个参数时适合

```

6、函数的`name`属性

```js
function f1(){}
f1.name 					// "f1"

var f2 = function(){}
f2.name						//"f2"

var f3 = function f4(){}
f3.name           // "f4"

f5 = new Function()
f5.name           //"anonymous"(匿名)
```

### 函数的this和arguments

函数本质是：调用call；用于反复调用的代码块。

`f`：函数对象；`f.call()`：执行这个函数体。

```js
f.call(asThis,input1,input2)
//其中asThis 会被当做this，[input1，input2]会被当做arguments
```

其中`f.call`的第一个参数可以用`this`得到；`f.call`的第二个以及后面的参数可以用`arguments`得到，并以伪数组形式呈现。

```js
f.call(undefined,'x','y','z'){
  console.log(arguments) //['x,'y','z']以伪数组形式呈现
  console.log(this)      //在普通模式下，this是undefined，浏览器会指向window 
 }                       //在'use strict'严格模式下，this就是undefined
```

### 函数的调用栈(Call Stack)

JavaScript的函数调用栈(Call Stack)其实就是一种解析器去处理程序的机制，它是栈数据结构。它能追踪子程序的运行状态。当函数调用的时候，就把该函数push到调用栈，结束时候，就从栈顶端移除。遵循FILO（先进后出）原则。另外当递归压栈时，容易造成`stack overflow`。

[这个网站](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D) 可以可视化帮助理解调用栈。

### 函数的作用域(Function Scope)

[作用域（scope）指的是变量存在的范围。在 ES5 的规范中，JavaScript 只有两种作用域：一种是全局作用域，变量在整个程序中一直存在，所有地方都可以读取；另一种是函数作用域，变量只在函数内部存在。ES6 又新增了块级作用域。](https://wangdoc.com/javascript/types/function.html#%E5%87%BD%E6%95%B0%E4%BD%9C%E7%94%A8%E5%9F%9F)

JavaScript 语言特有的"链式作用域"结构（chain scope），子对象会就近原则，一级一级地向上寻找最近父对象的变量。与全局作用域一样，函数作用域内部也会产生“变量提升”现象。

立即执行函数(IIFE)：是一个在定义时就会立即执行的JavaScript函数。当函数变成立即执行的函数表达式时，表达式中的变量不能从外部访问。

```js
//使用方法
( function(){/* code */}() );
!function () { /* code */ }();
~function () { /* code */ }();
-function () { /* code */ }();
+function () { /* code */ }();

```

闭包：

```js
!function(){
  var local = 'xxx'
  function foo(){
    console.log(local)
  }
}();
```

```js
function foo(){
  var local = 1
  function bar(){
    local++
    return local
  }
  return bar
}

var func = foo() //func现在是一个闭包
func()
```

以上两组代码都形成了一个闭包：**「函数」和「函数内部能访问到的变量」（也叫环境）的总和，就是一个闭包。**

闭包的**用处**有，一个是可以读取函数内部的变量，一个就是让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在，不会在调用结束后，被垃圾回收机制回收。另一个用处，是封装对象的私有属性和私有方法。

由于外层函数每次运行，都会生成一个新的闭包，而这个闭包又会保留外层函数的内部变量，所以内存消耗很大。因此不能滥用闭包，否则会造成网页的性能问题。



### 参考引用：

[阮一峰的JavaScript](https://wangdoc.com/javascript/types/function.html)

[「每日一题」JS 中的闭包是什么？](https://zhuanlan.zhihu.com/p/22486908)



