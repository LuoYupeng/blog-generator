---
title: 学习jQuery
date: 2019-03-25 22:57:33
tags:
---



jQuery的基本设计思想和主要用法，就是**"选择某个网页元素，然后对其进行某种操作"**。

## jQuery选择器

```js
$('#id');$('.class');$('span')
$('div > p') //<div> 元素的直接子元素的所有 <p> 元素
$('div p')   //<div> 元素的后代的所有 <p> 元素
$('span.red'); // 找出<span class="red ...">...</span>
```

```html
<ul>
    <li>1</li>
    <li>2</li>
</ul>

// $('li') 是一个伪数组的jQuery对象，{
                                   0: li,
                                   1: li,
 																	 legth: 2}
它的原型（共享属性）为 jQuery.prototype
```

```js
<div id='xxx'></div>

var div = document.getElementById('xxx')
var $div =$('#xxx')

//$(div) 可以将 div 封装成一个 jQuery 对象，就跟 $div 一样
//$div[0] === div ，$div 的第一项就是 div

//div 和 $div 的区别是：
//div为DOM对象，属性和方法有 childNodes firstChid nodeType 等
//$div为jQuery对象，属性和方法有 addClass removeClass toggleClass 等
//分清 DOM 和 jQuery 对象,他们都只拥有自己对象的方法
```

## jQuery常用API

`.get()`:通过`jQuery`对象获取一个对应的DOM元素，返回的是一个DOM对象

`.eq()`:返回为指定的索引的哪一个元素，返回的是一个`jQuery`对象

`addClass()`;`removeClass()`

`.toggleClass()`//如果在元素中指定类名称不存在，则添加指定类名称；如果元素已经拥有指定
类名称，则从元素中删除指定类名称。

```html
<div></div><div></div>

 $("<p></p>/>")
   .appendTo("div")  //将匹配的元素插入到目标元素内部的最后面
   .addClass("test")
   .end()
   .addClass("test2");
   
//结果：
<div><p class="test test2"></p></div>
<div><p class="test"></p></div>
```



## 自制一个jQuery

`jQuery`实质上是一个构造函数，该构造函数接受一个参数，jQuery通过这个参数利用原生API找到节点，之后返回一个方法对象，该方法对象上的方法对节点进行操作(方法使用了闭包)。

通过自制拥有几个简易功能的`jQuery`，来加深理解`jQuery`。

要求：

>```js
>window.jQuery = ???
>window.$ = jQuery
>
>var $div = $('div')
>$div.addClass('red') // 可将所有 div 的 class 添加一个 red
>$div.setText('hi') // 可将所有 div 的 textContent 变为 hi
>```

ⅰ. 获取元素节点,并返回一个纯净的伪数组。

```js
window.jQuery = function (nodeOrSelector) {
            let nodes = {}  
            if (typeof nodeOrSelector === 'string') {
              let temp = document.querySelectorAll(nodeOrSelector)
              for (let i = 0; i < temp.length; i++) {
                nodes[i] = temp[i]
              }
              nodes.length = temp.length
            } else if (nodeOrSelector instanceof Node) {
              nodes = {
                0: nodeOrSelector,
                length: 1
              }
            }
  
  return node
}
```



ⅱ. 添加`addClass()`方法

```js
nodes.addClass = function (className) {
                for (let i = 0; i < nodes.length; i++) {
                  nodes[i].classList.add(className)
                }
```



ⅲ. 添加`setText()`方法：

```js
nodes.Text = function (text) { 
              if(text === undefined){
                var texts = []
                for(i =0;i<nodes.length; i++){
                  texts.push(nodes[i].textContent)
                }
                return texts
              }else {
                for (let i =0;i<nodes.length;i++){
                  nodes[i].textContent = text
                }
              }
            }
```

组合一起就是：

```js
window.jQuery = function (nodeOrSelector) {
            let nodes = {}
            if (typeof nodeOrSelector === 'string') {
              let temp = document.querySelectorAll(nodeOrSelector)
              for (let i = 0; i < temp.length; i++) {
                nodes[i] = temp[i]
              }
              nodes.length = temp.length
            } else if (nodeOrSelector instanceof Node) {
              nodes = {
                0: nodeOrSelector,
                length: 1
              }
            }
  
            nodes.addClass = function (className) {
                for (let i = 0; i < nodes.length; i++) {
                  nodes[i].classList.add(className)
                }
            
            nodes.setText = function (text) { 
              if(text === undefined){
                var texts = []
                for(i =0;i<nodes.length; i++){
                  texts.push(nodes[i].textContent)
                }
                return texts
              }else {
                for (let i =0;i<nodes.length;i++){
                  nodes[i].textContent = text
                }
              }
            }
            return nodes
          }
```



这里是实现的思路：

第一版:

```js
function addClass(node1, className){
  var node = node1
  node.classList.add(className)
}
function setText(node1, value){
  var node = node1
  node.innerText = value
}
```

第二版:

```js
window.jQuery = function(node1){
  var node = node1
  
  node.addClass = function(className){
  	node.classList.add(className)
  }
  
  node.setText = function(value){
  	node.innerText = value    
  }
  
  return node
}
```

第三版: 支持输入字符串

```js
window.jQuery = function(node1){
  var node
  
  if(typeof node1 === "string"){
    node = document.querySelector(node1)
  }else{
    node = node1
  }
  
  node.addClass = function(className){
  	node.classList.add(className)
  }
  
  node.setText = function(value){
  	node.innerText = value    
  }
  
  return node
}
```

第四版: 支持操作多个元素

```js
window.jQuery = function(node1){
  var node
  
  if(typeof node1 === "string"){
    node = document.querySelectorAll(node1)
  }else{
    node = {}
    node[0] = node
    node["length"] = 1
  }
  
  node.addClass = function(className){
  	for(var i = 0; i < node.length; i++){
      node[i].classList.add(className)
  	}
  }
  
  node.setText = function(value){
    for(var i = 0; i < node.length; i++){
    	node[i].innerText = value
    }
  }
  
  return node
}
```

第五版: 通过jQuery返回的是纯净的伪数组

```js
window.jQuery = function(node1){
  var node = {}
  
  if(typeof node1 === "string"){
    var temp = document.querySelectorAll(node1)
    for(var j = 0; j < temp.length; j++){
      node[j] = temp[j]
    }
    node["length"] = j
  }else{
    node = {}
    node[0] = node
    node["length"] = 1
  }
  
  node.addClass = function(className){
  	for(var i = 0; i < node.length; i++){
      node[i].classList.add(className)
  	}
  }
  
  node.setText = function(value){
    for(var i = 0; i < node.length; i++){
    	node[i].innerText = value
    }
  }
  
  return node
}
```

