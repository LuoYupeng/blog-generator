---
title: 初识MVC
date: 2019-03-29 20:29:50
tags:
---

### 前端的MVC是什么

MVC 架构模式是指 Model-View-Controller（数据层-视图层-控制器层）模式。**Model - 封装数据操作**，**View - 视图渲染**，**Controller - 控制器主要负责逻辑以及其他的**。

![](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015020105.png)

在MVC的基础上，还衍生出了MVP模式，MVVM模式。例如MVP模式（如下图）：

![MVP.jpg](https://i.loli.net/2019/04/04/5ca611a0a708f.jpg)

MVP 模式是MVC基础上，将 Controller 改名为 Presenter。MVP各部分之间的通信，都是双向的，View与Model之间不直接通信。

### MVC的尝试使用

#### 代码模块化

​	代码模块化是指，将一堆功能的代码，按照各自功能抽离到独立js文件，使用立即执行函数包裹所有代码，不暴露全局变量。

#### 以创建一个留言区为例

![留言.jpg](https://i.loli.net/2019/04/04/5ca620dc931d9.jpg)

HTML:

```HTML
<section  class="message" >
    <h2>留言</h2>
    <ol id="messageList">
    </ol>
    <form id="postMessageForm" >
      <label for="">姓名:</label>
      <input type="text" name="name">
      <label for="">内容:</label>
      <input type="text" name="content">
      <input type="submit" value="提交">
    </form>
  </section>
```



JS:

1.0版本：

```js
//初始化LeanCloud数据库
var APP_ID = '*******************'; //每个LeanCloud应用的ID和KEY都是私有独立的
var APP_KEY = '************************'; //

AV.init({
    appId: APP_ID,
    appKey: APP_KEY
});
//上传留言
let myForm = document.querySelector('#postMessageForm')
myForm.addEventListener('submit',function (e) {
    e.preventDefault()
    let content = myForm.querySelector('input[name=content]').value
    var Message = AV.Object.extend('Message')
    var message = new Message()
    message.save({
        'content' : content
    }).then(function (object) {
        alert('留言成功')
    })
})
```

在这里，采用[LeanCloud](<https://leancloud.cn/>)作为我们的一个后台数据库储存数据。LeanCloud有开发版免费使用，在请求订阅低的

时候，个人demo或者小项目都可以采用。

1.01版本：可以从数据库读取留言并展示出来

```js
var query = new AV.Query('Message')
query find() //展示数据库存储的留言
    .then(
        function (messages) {
            let  array = messages.map((item)=> item.attributes)
            array.forEach( (item)=> {
                let li = document.createElement('li')
                li.innerText= item.content
                let messageList = document.querySelector('#messageList')
                messageList.appendChild('li')
            })
        }
    )
let myForm = document.querySelector('#postMessageForm') //点击留言功能
myForm.addEventListener('submit',function (e) {
    e.preventDefault()
    let content = myForm.querySelector('input[name=content]').value
    var Message = AV.Object.extend('Message')
    var message = new Message()
    message.save({
        'content' : content
    }).then(function (object) {
        alert('留言成功')
    })
})
```

1.02版本:即时免刷新显示用户留言

```js
var query = new AV.Query('Message');
query.find()  //展示数据库存储的留言
    .then(
        function (messages) {
            let  array = messages.map((item)=> item.attributes).reverse()
            array.forEach( (item) => {
                let li = document.createElement('li')
                li.innerText= `${item.name}: ${item.content}`
                let messageList = document.querySelector('#messageList')
                messageList.appendChild(li)
            })
        }
    )
let myForm = document.querySelector('#postMessageForm')  //点击留言功能
myForm.addEventListener('submit',function (e) {
    e.preventDefault()
    let content = myForm.querySelector('input[name=content]').value
    let name = myForm.querySelector('input[name=name]').value
    var Message = AV.Object.extend('Message')
    var message = new Message()
    message.save({
        'name': name,
        'content' : content
    }).then(function (object) { //假刷新功能，即时免刷新将用户留言展现出来
        let li = document.createElement('li')
        li.innerText= `${object.attributes.name}: ${object.attributes.content}`
        let messageList = document.querySelector('#messageList')
        messageList.prepend(li)
        myForm.querySelector('input[name=content]').value = '' //点击提交后清空留言内容
    })
})
```

1.10版本：用MVC模式封装代码

```js
!function () {
  //MVC的M
    var model = {
        //获取数据
        init: function () {
            var APP_ID = '4sAEIQMOAHFjjEs525eqbRHC-9Nh9j0Va';
            var APP_KEY = 'mAiD9Kb9q74Uo9EWA8gi1fv9';

            AV.init({
                appId: APP_ID,
                appKey: APP_KEY
            })
        },
        fetch: function () {
            var query = new AV.Query('Message');
            return query.find()
        },
        //创建数据
        save: function (name, content) {
            var Message = AV.Object.extend('Message')
            var message = new Message();
            return message.save({
                'name': name,
                'content': content
            })
        }
    }
    //MVC的V
    var view = document.querySelector('section.message')
    //MVC的C
    var controller = {
        view: null,
        model: null,
        messageList: null,
        init: function (view, model) {
            this.view = view
            this.model = model
            this.messageList = view.querySelector('#messageList')
            this.form = view.querySelector('form')
            this.model.init()
            this.loadMessages()
            this.bindEvents()
        },

        loadMessages: function () {
            this.model.fetch().then(
                (messages) => {
                    let array = messages.map((item) => item.attributes).reverse()
                    array.forEach((item) => {
                        let li = document.createElement('li')
                        li.innerText = `${item.name}: ${item.content}`
                        this.messageList.appendChild(li)
                    })
                }
            )
        },
        bindEvents: function () {
            this.form.addEventListener('submit',  (e)=> {
                e.preventDefault()
                this.saveMessage()
            })
        },
        saveMessage: function () {
            let myForm = this.form
            let content = myForm.querySelector('input[name=content]').value
            let name = myForm.querySelector('input[name=name]').value
            this.model.save(name, content).then(function (object) {
                let li = document.createElement('li')
                li.innerText = `${object.attributes.name}: ${object.attributes.content}`
                let messageList = document.querySelector('#messageList')
                messageList.prepend(li)
                myForm.querySelector('input[name=content]').value = ''
            })
        }
    }

    controller.init(view, model)
}.call()
```

**MVC里的套路**，在MVC里有一些固定的模板格式，基本遵循下图所示：

![MVC格式套路.jpg](https://i.loli.net/2019/04/05/5ca62eabe6f59.jpg)

#### 面向对象编程(OOP)

当面对大型项目时，一堆的MVC，可以把MVC里面的model，view，controller各自提取出一个模板(函数)封装起来，把需要修改的地方作为参数传进去。

wiki解释：

> **面向对象程序设计**（英语：Object-oriented programming，[缩写](https://zh.wikipedia.org/wiki/%E7%BC%A9%E5%86%99)：OOP）是种具有[对象](https://zh.wikipedia.org/wiki/%E5%AF%B9%E8%B1%A1_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))概念的[程序编程典范](https://zh.wikipedia.org/wiki/%E7%BC%96%E7%A8%8B%E8%8C%83%E5%9E%8B)，同时也是一种程序开发的抽象方针。它可能包含[数据](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE)、[属性](https://zh.wikipedia.org/w/index.php?title=%E5%B1%9E%E6%80%A7_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6)&action=edit&redlink=1)、[代码](https://zh.wikipedia.org/wiki/%E6%BA%90%E4%BB%A3%E7%A0%81)与[方法](https://zh.wikipedia.org/wiki/%E6%96%B9%E6%B3%95_(%E9%9B%BB%E8%85%A6%E7%A7%91%E5%AD%B8))。对象则指的是[类](https://zh.wikipedia.org/wiki/%E7%B1%BB_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))的实例。它将[对象](https://zh.wikipedia.org/wiki/%E7%89%A9%E4%BB%B6_(%E9%9B%BB%E8%85%A6%E7%A7%91%E5%AD%B8))作为[程序](https://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A8%8B%E5%BA%8F)的基本单元，将程序和[数据](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE)[封装](https://zh.wikipedia.org/wiki/%E5%B0%81%E8%A3%9D_(%E7%89%A9%E4%BB%B6%E5%B0%8E%E5%90%91%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88))其中，以提高软件的重用性、灵活性和扩展性，对象里的程序可以访问及经常修改对象相关连的数据。在面向对象程序编程里，计算机程序会被设计成彼此相关的对象。

MDN上给了一堆术语：

>- Namespace 命名空间
>
>  允许开发人员在一个独特，应用相关的名字的名称下捆绑所有功能的容器。
>
>- Class 类
>
>  定义对象的特征。它是对象的属性和方法的模板定义。
>
>- Object 对象
>
>  类的一个实例。
>
>- Property 属性
>
>  对象的特征，比如颜色。
>
>- Method 方法
>
>  对象的能力，比如行走。
>
>- Constructor 构造函数
>
>  对象初始化的瞬间，被调用的方法。通常它的名字与包含它的类一致。
>
>- Inheritance 继承
>
>  一个类可以继承另一个类的特征。
>
>- Encapsulation 封装
>
>  一种把数据和相关的方法绑定在一起使用的方法。
>
>- Abstraction 抽象
>
>  结合复杂的继承，方法，属性的对象能够模拟现实的模型。
>
>- Polymorphism 多态
>
>  多意为「许多」，态意为「形态」。不同类可以定义相同的方法或属性。

我们在写代码时，当完成了一个需求后，我们就需要对我们的代码中不断地进行解耦、抽象抽离、封装接口，将重复的代码、功能性的代码进行封装，“为变化服务”！









**参考文章：**

[MVC，MVP 和 MVVM 的图示](<http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html>)

[每日一题」MVC 是什么？](<https://zhuanlan.zhihu.com/p/22943208>)