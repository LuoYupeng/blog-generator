---
title: DOM学习
date: 2019-03-05 20:31:06
tags:
---

# DOM树

DOM(Document Object Model)：文档对象模型。

由于HTML文档被浏览器解析后就是一棵DOM树，要改变HTML的结构，就需要通过JavaScript来操作DOM。

<img src="https://i.loli.net/2019/03/25/5c97c864e907b.png" alt="DOM树.png" width="600px" title="DOM树.png" />

上图就是DOM树，DOM 的最小组成单位叫做节点（node）其中每个节点的都是**Node类型**：

- `Document`：整个文档树的顶层节点

- `DocumentType`：`doctype`标签（比如`<!DOCTYPE html>`）

- `Element`：网页的各种HTML标签（比如`<body>`、`<a>`等）

- `Attribute`：网页元素的属性（比如`class="right"`）

- `Text`：标签之间或标签包含的文本

- `Comment`：注释

- `DocumentFragment`：文档的片段

  

所有DOM节点对象都继承了Node接口，这是 DOM 操作的基础。

## Node接口的属性和方法

对于DOM的操作，最主要归纳于四个字：**增、删、改、查**。

首先是**查:**

`Document.getElementsByClassName() `;//返回指定Class name的节点

`Document.getElementById()`;//返回指定ID的节点

`Document.querySelector()`;//接受一个 CSS 选择器作为参数，返回第一个匹配该选择器的元素节点

`Document.querySelectorAll()`;//返回一个`NodeList`对象，包含所有匹配给定选择器的节点。



//两种方法获取的节点是有有区别的，`document.querySelectorAll`方法返回的是一个静态集合。DOM内部的变化，并不会实时反映在该方法的返回结果之中。

<img src="https://i.loli.net/2019/03/25/5c97d4dbe986e.png" width='600px' alt="getElementBy与querySelector区别.png" title="getElementBy与querySelector区别.png" />

在得到相应Node节点时候， Node节点本身也有一些**重要属性**：

`nodeName`//返回节点类型,重要的返回的值有`大写的HTML元素名`, `#text `,`#document`

`nodeType`//属性返回一个整数值，表示节点的类型。文档节点（document）：9，元素节点(element)：1；文本节点(text)：3

`nodeValue`//返回一个字符串，表示当前节点本身的文本值，只有文本节点（text）、注释节点（comment）和属性节点（attr）有返回结果，其他节点都返回null

`textContent`//属性自动忽略当前节点内部的 HTML 标签，返回所有文本内容。//而`innerText`在IE下会忽略隐藏(例如css,style)，同时也会重置写入。

兄弟关系：`Node.nextSibling`;`Node.previousSibling`//会获取到文本节点(例如回车)

父子关系：`node.parentNode`;`node.childNodes`;`node.firstChild`;`node.lastChild`

其中`node.childNodes`属性返回一个伪数组的对象（`NodeList`集合）；伪数组内的值是动态变化的，回车也会充当文本体现出来。

**Node方法**：

- `Node.appendChild()`//将其作为最后一个子节点，插入当前节点
- `Node.hasChildNodes()`//返回一个布尔值，表示当前节点是否有子节点
- `Node.cloneNode()`//克隆一个节点，接受参数true为表示同时克隆子节点
- `Node.insertBefore(newNode, referenceNode)`//将某个节点插入父节点内部的指定位置。
- `Node.removeChild()`用于从当前节点移除该子节点,被移除的节点依然存在于内存之中，但不再是 DOM 的一部分。
- `Node.replaceChild(newChild, oldChild)`//替换当前节点的某一个子节点
- `Node.contains()`
- `Node.isEqualNode()`//用于检查两个节点是否相等,指的是两个节点的类型相同、属性相同、子节点相同。
- `Node.isSameNode()`//表示两个节点是否为同一个节点。
- `Node.normalize()`

# Document节点的属性和方法：

Document节点属性有：

**document.hidden**返回一个布尔值，表示当前页面是否可见。配合**document.visibilityState**(返回文档的可见状态)使用。

**document.referrer**表示当前文档的访问者来自哪里，可用于网站图片防盗链，流量，DDOS。

+ **document.characterSet**；**document.title**；**document.domain**；**document.styleSheets**；**document.scripts**；**document.plugins**
+ **document.images**;**document.links**;**document.forms**;
+ **document.doctype**;**document.documentElement**//返回HTML;
+ **document.body**；**document.head**；**document.scrollingElement**；**document.fullscreenElement**

### Document节点方法有：

####  DOM的**增**：

Document.createElement() // 创建元素

Document.createTextNode() //创建一个新的文本节点

Node.appendChild()//插入当前节点

Element.classList.add() //添加指定的类值class

Document.write();Document.writeIn() // 将文本字符串写入打开的文档流；document.write()和writeIn()的区别是前者没有换行，而后者有换行。但要防止异步、setTimeOut导致冲洗掉整个文档。

#### DOM的**删**：

Element.removeAttribute() // 从元素中删除指定的属性

Element.removeChild(）// 删除子元素

ChildNode.remove() // 删除元素

Child.parentNode.removeChild(child) // 不确定父元素时可这样删除子元素

Element.classList.remove() // 移除元素中一个或多个类名

#### DOM的改：

Node.replaceChild() // 替换子节点

style.property = new style // 修改元素CSS属性值

Element.setAttribute() // 设置或者改变指定属性并指定值

Node.innerText // 修改元素文本内容

Element.innerHTML // 设置或获取描述元素后代的HTML语句



# DOM事件模型

在2000年11月，DOM标准在Level 2中加入了Events事件模型，后期的Level 3也并未对Events进行修改。

以事件监听为例：

```js
<button id="xxx">点击</button>

xxx.addEventListener('chick',function(){ 
        alert('我被点击了')})
//.addEventListener()方法，可以增加多次事件监听，以队列形式触发，先进先出。

xxx.onclick = function(){
  alert('我被点击了')
}
//.onclick方法：属性唯一，只能有一个，多个会覆盖前面。

.one()//只执行一次，然后把自己从事件队列移除。
```

DOM事件流包括三个阶段。

1. 事件捕获阶段

2. 处于目标阶段

3. 事件冒泡阶段

   <img src="https://i.loli.net/2019/03/25/5c97f1c2993c7.png" alt="DOM事件流.png" title="DOM事件流.png" />

   `xxx.addEventListener('chick',function(){},false)`: 事件监听默认为false，冒泡阶段触发。当值为`true`时，捕获阶段触发；处于捕获阶段的事件会比冒泡阶段先发生。

   <img src="https://i.loli.net/2019/03/25/5c97f58282f39.png" alt="DOM事件流1.png" height="300px" title="DOM事件流1.png" />

   

<img src="https://i.loli.net/2019/03/25/5c97f5828c394.png" alt="DOM事件流2.png" height="300px" title="DOM事件流2.png" />

### [实现一个点击弹出popover](<http://js.jirengu.com/zesar/6/edit?html,js,console,output>)

要求

1. 点击按钮弹出浮层
2. 点击别处关闭浮层
3. 点击浮层时，浮层不得关闭
4. 再次点击按钮，浮层消失

<img src="https://i.loli.net/2019/03/25/5c97f79410c39.png" alt="点击popover.png" height="300px" title="点击popover.png" />













参考文献：

[Javascript DOM 元素的增删改查](<https://zhuanlan.zhihu.com/p/27297443>)

[对 DOM 的一些理解](<https://github.com/wojiaofengzhongzhuifeng/study/blob/master/blog/B8-DOM.md>)

[廖雪峰-操作DOM](<https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/001434499851683f7f8d6b7717343248a75d5e7f930def4000>)

[阮一峰教程-DOM](<https://wangdoc.com/javascript/dom/index.html>)







​          