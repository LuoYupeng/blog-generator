---
title: JS的数据类型转换
date: 2018-12-19 02:32:29
tags:
---

JavaScript 是一种动态类型语言，变量没有类型限制，可以随时赋予任意值。

## 1数据类型的转换

### 1.1Number()

使用`Number`函数，可以将任何类型的值转化为数值。

```js
Number(123)     // 123
Number('123')  //123
Number('123abc') // NaN

Number(true) // 1
Number(false) // 0

Number(undefined) // NaN
Number(null) // 0
```

使用parseInt()函数，可以将值转化为整数值，使用parseFloat()函数，可以将值转化为浮点数。

```js
parseInt('12.3')  //12
parseInt('12a3c') //12
parseInt(1101,2) //13,表示将二进制数字1101转换成十进制

parseFloat('12.3') //12.3
```

在实际使用中，很多选择这种方式进行转换

```js
'123' - 0  //123
+'123'     //123
```

### 1.2Boolean() 

Boolean函数的转换规则相对简单，除以下五个值转换结果为`false`，其他都为`true`。

> undefined
>
> null
>
> -0 或+0
>
> NaN
>
> ' ' (空字符串)

所有对象，包括空对象的转换结果都是`true`。

### 1.3String()

`String`函数可以将任意类型的值转化成字符串，转换规则如下。

> 数值：转为相应的字符串。
> 字符串：转换后还是原来的值。
> 布尔值：true转为字符串"true"，false转为字符串"false"。
> undefined：转为字符串"undefined"。
> null：转为字符串"null"。

```js
String({a: 1}) // "[object Object]"
String([1, 2, 3]) // "1,2,3"
```

`number.toString()`函数可以使用不同进制把一个数字转换为字符串。

```js
15.toString() //15，默认将数字15转为十进制后输出字符串
15.toString(2) //1111，将数字15转为二进制后输出字符串
15.toString(8) //17
15.toString(16) //f
```

在实际使用中，可以`+''`来时实现转换。

```js
123 + '' //'123'
true + '' // 'true'
obj + '' // '[obj obj]'
123 + '12' // '12312'
```

## 2各数据类型的内存存储

内存数据区存储分为Stack栈内存和Heap堆内存。

简单类型的值储存在Stack栈内存里，数字以64位储存，字符串以16位储存。

复杂类型将值存储在Heap堆内存，在Stack栈内存上记录其Heap堆内存地址。

####2.1浅拷贝和深拷贝

对于简单类型的数据来说，赋值就是深拷贝。
对于复杂类型的数据（对象）来说，才要区分浅拷贝和深拷贝。深拷贝是对 Heap 内存进行完全的拷贝。

下面有几道常见面试题，在解答时只需要画出简略内存图就可以明知答案。

```js
// 一
var a = 1
var b = a
b = 2
a // 1

// 二
var a = {name: 'a'}
var b = a
b = {name: 'b'}
a.name //'a'

// 三
var a = {name: 'a'}
var b = a
b.name = 'b'
a.name  // 'b'

// 四
var a = {name: 'a'}
var b = a
b = null
a   // {name: 'a'}
```



参考文献：

[阮一峰的JavaScript教程](https://javascript.ruanyifeng.com/grammar/conversion.html#toc5)

