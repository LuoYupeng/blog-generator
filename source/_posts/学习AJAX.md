---
title: 学习AJAX
date: 2019-03-27 15:24:52
tags:
---

## AJAX是什么

**AJAX**(Asynchronous JavaScript and XML)：异步的JavaScript和XML。用 JS 发起一个请求，并得到服务器返回的内容。异步请求数据，局部更新页面内容。

#### window.XMLHttpRequest()

1. 使用XMLHttpRequest发送请求
2. 服务器返回XML格式的字符串(XML后来用JSON替代)
3. JS解析XML，并局部更新页面

#### 一个简易的AJAX发请求

```js
myButton.addEventListener('click',(e) => {
    let request = new XMLHttpRequest()
    request.open('GET','./xxx') //配置request
    request.send()   
    request.onreadystatechange =  ()=>{
        if(request.readyState === 4){
            console.log('请求响应都完毕了')
            if(request.status >= 200 && request.status <= 300){
                console.log ('说明请求成功')               
                let string =request.responseText
                //把符合的JSON语法的字符串 转换成JS对应的值
                let object = window.JSON.parse(string)
             
            }else if (request.status >= 400) {
                console.log('请求失败')
            }
        }
    }
})
```

#### 同源策略和CORS跨域

![同源策略.png](https://i.loli.net/2019/03/27/5c9b3a36adfa5.png)

**同源策略**限制了从同一个源加载的文档或脚本如何与来自另一个源的资源进行交互。这是一个用于隔离潜在恶意文件的重要安全机制。目的是为了保证用户信息的安全，防止恶意的网站窃取数据。

所谓**同源**是指"协议+域名+端口"三者相同。

> 协议相同
> 域名相同
> 端口相同

**同源策略限制内容有：**

- Cookie、LocalStorage、IndexedDB 等存储性内容
- DOM 无法获得
- AJAX 请求发送后，结果被浏览器拦截(`request.status = 0`)

 **突破同源策略**：

- JSONP
- WebSocket
- CORS
- 架设服务器代理

**CORS跨域**：(Cross-Origin Resource Sharing)服务端设置`response.setHeader('Access-Control-Allow-Origin','http://(允许访问的域名)')`就可以开启 CORS。 该属性表示哪些域名可以访问资源，如果设置通配符则表示所有网站都可以访问资源。

`form表单提交`可以跨域是因为原页面用form提交到另外一个域名后，会自动刷新跳转另外一个域名，原来页面的脚本无法获得新页面的内容。

### 自己封装一个window.jQuery.ajax

先自己创建个jQuery

```js
window.jQuery = function (nodeOrSelector) {
    let nodes = { }
    nodes.addClass = function(){}
    nodes.html = function () {}
    return nodes
}
window.$ = window.jQuery
```

1. 第一版

```js
window.jQuery.ajax = function (url, method, body, successFn, failFn) {
    let request = new XMLHttpRequest()
    request.open(method,url) //配置request 请求第一部分
    request.onreadystatechange =  ()=>{
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <= 300){
                successFn.call(undefined,request.responseText)
            }else if (request.status >= 400) {
                failFn.call(undefined,request)
            }
        }
    }
    request.send(body)
}
```

```js
myButton.addEventListener('click',(e) => {
    window.jQuery.ajax(
        './xxx',
        'post',
        'a=1&b=2',
        (responseText) => {console.log(1)},
        (request) => {console.log(2)}
    )
})
```

2. 优化传参

```js
window.jQuery.ajax = function ({url, method, body, headers,successFn,failFn}) {
    request.open(method,url) //配置request 请求第一部分
    for(let key in headers){ //遍历设置多个header
        let value = headers[key]
        request.setRequestHeader(key, value)
    }
    request.onreadystatechange =  ()=>{
        if(request.readyState === 4){
            if(request.status >= 200 && request.status <= 300){
                successFn.call(undefined,request.responseText)
            }else if (request.status >= 400) {
                failFn.call(undefined,request)
            }
        }
    }
    request.send(body)
}

```

```js
myButton.addEventListener('click',(e) => {
    window.jQuery.ajax({
        url: './xx',
        method: 'get',
        headers: {
            'content-type': 'application/x-www-form-urlencoded',
            'tom': '18'
        },
        successFn: (x)=>{
            f1.call(undefined,x) //成功后执行两个函数function f1(responseText){}；
            f2.call(undefined,x) // function f2(responseText){}
        },
        failFn: (x)=>{
            console.log(x)
            console.log(x.status)
            console.log(x.responseText )
        }
    })
})
```

3. 添加Promise功能

```js
window.jQuery.ajax = function ({url, method, body, headers}) {
  	return new Promise(function (resolve, reject) {
        let request = new XMLHttpRequest()
        request.open(method, url) //配置request 请求第一部分
        for (let key in headers) { //遍历设置多个header
            let value = headers[key]
            request.setRequestHeader(key, value)
        }
        request.onreadystatechange = () => {
            if (request.readyState === 4) {
                if (request.status >= 200 && request.status <= 300) {
                    resolve.call(undefined, request.responseText)

                } else if (request.status >= 400) {
                    reject.call(undefined, request)
                }
            }
        }
        request.send(body)
    })
}
```

```js
myButton.addEventListener('click',(e) => {
    window.jQuery.ajax({
        url: './xxx',
        method: 'get',
        headers: {
            'content-type': 'application/x-www-form-urlencoded',
            'tom': '18'
        }
    }).then(
        (text) => {console.log(text)},
        (request) => {console.log(request)}
    )
// let promise = .... //promise.then()
})
```

