---
title: HTTP请求和响应入门
date: 2018-10-26 01:31:41
tags: HTTP
---
HTTP_(HyperText Transfer Protocol)_ ，译为超文本传输协议，目前常用HTTP协议版本为HTTP/1.1和HTTP/1.2。
当用户在浏览器输入网址点击回车后，大致发生了以下流程：
<img src="https://i.loli.net/2018/11/28/5bfd9061f3a44.png" alt="20181128024212.png" title="20181128024212.png" >
网页地址URL格式是: 
> _协议_ :// _域名端口_ / _路径_?_查询参数_/_锚点_  

# HTTP请求
HTTP请求报文分为三个部分，报文首部/空行/报文主体。

|    |结构|举例
|----|-----|-----|
|请求行|方法 路径 协议/版本号| GET /s?wd=123 HTTP/1.1|
|各种首部字段|key:value|Connection: keep-alive <br> Cookie: xxx|
|空行| 空行CR+LF |空行  |
|报文主体|应被发送的数据|例如登录密码 |
（路径包含查询参数，但不包含锚点）
在Chrome开发者工具中，Network下可以查看请求报文首部和响应报文首部。
<img src="https://i.loli.net/2018/11/28/5bfd90683aac8.png" alt="20181128024239.png" title="20181128024239.png" width="50%" height="50%">
点击`view source`可以得到如下显示
<img src="https://i.loli.net/2018/11/28/5bfd90686cde5.png" alt="20181128024146.png" title="20181128024146.png" width="50%" height="50%">
## HTTP请求方法
HTTP的请求动作方法有以下几种

|方法|说明|方法|说明|
|---|---|---|---|
|GET|获取资源|POST|传输实体|
|PUT|传输文件|HEAD|获得报文首部|
|DELETE|删除文件|OPTIONS|询问支持的方法|
|TRACE|追踪路径|CONNECT|要求用隧道协议连接代理
|LINK|建立和资源之间的联系|UNLINK|断开连接关系|

# HTTP响应
在上文可以知道，HTTP响应也可以在Chrome开发者工具获取，HTTP响应报文同样分为三个部分，报文首部/空行/报文主体。

|    |结构|举例
|----|-----|-----|
|状态行|协议/版本号 状态码 解释短语| HTTP/1.1 200 OK|
|各种首部字段|key:value|Content-Type: text/html <br> Content-Length: 362|
|空行| 空行CR+LF |空行  |
|报文主体|要下载的内容|例如HTML |
## HTTP状态码
状态码的职责是当客户端向服务端发送请求时，描述返回的请求结果。借助状态码，用户可以知道服务器端是正常处理了需求，还是出现了错误。

| |类别|原因短语|
|---|---|
|1xx|Information（信息性状态码）|接收的请求正在处理|
|2xx|Success（成功状态码）|请求正常完成|
|3xx|Redirection（重定向状态码）|需要进行附加操作以完成请求|
|4xx|Client Error（客户端错误状态码）|服务器无法处理请求|
|5xx|Server Error（服务器错误状态码）|服务器处理请求出错|
例如：200表示成功；301永久重定向；302临时重定向；403客户端不允许访问；404服务器没有请求的资源；500服务器内部错误。
# `curl`命令
在浏览器上我们可以直接输入网址，但在命令行中，同样可以通过`curl`命令来完成动作。在官方文档对`curl`的解释是：`curl`是一种使用其中一种支持的协议从服务器传输数据或向服务器传输数据的工具，`curl`提供了大量有用的技巧，如代理支持，用户身份验证，FTP上传，HTTP发布，SSL连接，cookie，文件传输恢复等，功能的数量将使你的头旋脑转！
> `curl`语法 
>  curl  [options]  [URL...]

>curl -s -v -H "key: value" -- "www.baidu.com"
>-s:表示安静模式，不显示进度条；-v：表示显示请求报文和响应报文；-H “key：alue”：往首部字段添加key/alue

>curl -X POST -d "xxxx" -s -v -H "key: value" -- "www.baidu.com"
>-X POST:表示改变请求方法为POST（或者其他）； -d “xxxx”：表示上传xxxx

在这篇文章中，还省略了同样重要复杂的首部字段，分别有通用首部、请求首部、响应首部、实体首部及其他首部字段。

参考文献：
* [图解HTTP](http://www.ituring.com.cn/book/1229)
* [精读《图解HTTP》](https://github.com/bailinlin/Awsome-Front-End-Xmind/issues/5)

    <strong>Fighting</strong>


