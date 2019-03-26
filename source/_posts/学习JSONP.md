---
title: 学习JSONP
date: 2019-03-26 20:54:14
tags:
---

### JSONP是什么

请求方：tom.com的前端程序员(浏览器)

相应方：jerry.com的后端程序员(服务器)

1. 请求方动态创建`script`，src指向响应方，同时传一个查询参数`?callbackName=yyy`
2. 响应方根据查询参数`callbackName`，构形如`yyy.call(undefined,'你要的数据')`这样的响应
3. 请求方接收到响应后，执行`yyy.call`，如此就得到需要的数据

##### JSONP为什么不能使用`post`

答：因为JSONP是通过动态创建`script`来实现的，动态创建`script`只能通过GET请求方法，不能使用POST请求方法。



以点击按钮扣一块为例：

```html
<h5>您的账户余额是 <span id="amount">&&&amount&&&</span></h5>
<!-- &&&amount&&& 是占位符-->
<button id="button">付款</button>
```



JSONP出现前的历史做法：(拥有`src`属性的标签都可以跨域发送请求)

1. 使用`form`表单提交`POST`请求，但是一定会刷新当前页面或者一个`iframe`页面。

   ```js
   <form ... target="result">
   <iframe name="result">
   ```

2.  局部刷新，不用`form`和`frame`时，使用`script`创建`img`发请求（1x1px空白像素图片），通过服务器返回的状态码来判断请求成功或失败。缺陷只能用GET，不能用POST。

   ```js
      // 图片发请求,js代码
        let image =document.createElement('img')
        image.src = '/pay'
        image.onload = function () {
            alert("成功")
           amount.innerText = amount.innerText - 1
        }
        image.onerror = function () {
            alert("失败")
        }
   ```

   ```js
   // 路由代码
   else if (path === '/pay' ){
         var amount = fs.readFileSync('./db','utf8') // 读数据 100
         var newAmount = amount - 1
         fs.writeFileSync('./db',newAmount)
         response.setHeader('Content-Type','image/png')
         response.statusCode = 200
         response.write(fs.readFileSync('./dog.png'))//返回一个图片
         response.end()
     } else{
       response.statusCode = 404
       response.setHeader('Content-Type', 'text/html;charset=utf-8')
       response.write('找不到对应路径，请修改')
       response.end()
     }
   ```

   

3. `script`发请求，然后appendChild到body上，响应结束后把`script`删除。

   ```js
   //js代码
   button.addEventListener('click', (e)=>{
      let script = document.createElement('script')
       script.src = '/pay'
       document.body.appendChild(script)
       script.onload = function(e){
          e.currentTarget.remove()
       }
   
       script.onerror = function () {
           alert('fail')
           e.currentTarget.remove()
       }
   ```

   ```js
   //路由代码
   else if (path === '/pay' ){
         var amount = fs.readFileSync('./db','utf8') // 100
         var newAmount = amount - 1
         fs.writeFileSync('./db',newAmount)
         response.setHeader('Content-Type','application/javascript')
         response.statusCode = 200
         response.write(`
                         amount.innerText = amount.innerText - 1`)
         response.end()
     } else{
   
   ```

   

## 写出一个JSONP

假设tom.com向jerry.com发送请求

```js
//tom.com
button.addEventListener('click', (e)=>{
        let script = document.createElement('script')
        let functionName = 'tom' + parseInt(Math.random() * 10000, 10)
        window[functionName] = function (result) {
            if (result === 'success') {             
                amount.innerText = amount.innerText - 1
            } else {
            }
        }
        script.src = 'http://jerry.com:8002/pay?callback=' + functionName
        document.body.appendChild(script)
        script.onload = function (e) {
            e.currentTarget.remove()
            delete window[functionName]
        }
        script.onerror = function () {
            alert('fail')
            e.currentTarget.remove()
            delete window[functionName]
        }
```

```js
//jerry.com的路由
else if (path === '/pay' ){
      var amount = fs.readFileSync('./db','utf8') // 100
      var newAmount = amount - 1
      fs.writeFileSync('./db',newAmount)
      response.setHeader('Content-Type','application/javascript')
      response.statusCode = 200
      response.write(`
                      ${query.callback}.call(undefined,'success')
                      `)
      //${query.callbackName}获取传参  把success用json表示时就是JSONP
      response.end()
```

jQuery已经封装好了相关函数，写法如下：

```js
button.addEventListener('click', (e)=>{
//jQuery写jsonp
    $.ajax({
        url: 'http://jerry.com:8002/pay',
        dataType: 'jsonp',
        success: function (response) {
            if(response === 'success'){
                //...code...//
            }
        }
    })
```



