---
title: 简述JS里的数据类型
date: 2018-12-18 01:35:39
tags:
---

JavaScript中的数据类型有：

   基本类型：Number，String，Boolean，Null，Undefined；

   复杂类型：Object；

   ES6中，新增了Symbol。

1、Number类型

根据ECMAScript标准，JS内部中是以64位浮点数形式存储数字，此外还有一些值：`+Infinity`；`-Infinity`；`NaN`。

由于浮点数不是精确的值，所以涉及到小数运算时要注意，例如：

```js
0.1 + 0.2 === 0.3
// false，实际是等于0.30000000000000004
```

JS的数值有多种表示方法，可以直接用字面形式表示，例如0b11（二进制）；21（十进制）；011（八进制）；0x11（十六进制）。数值也可以采用科学计数方法表示，例如`123e3 //表示123000`；`123e-3 //表示0.123`。

`NaN`表示“非数字”（Not a Number），主要出现在将字符串解析成数字出错的场合。需要注意的是`NaN === NaN // false`。

与数值相关的方法有：

* parseInt()方法：用于将字符串转为整数。如果字符串的第一个字符不能转化为数字（后面跟着数字的正负号除外），返回`NaN`。`parseInt`方法还可以接受第二个参数（2到36之间），表示被解析的值的进制，返回该值对应的十进制数。默认情况下，`parseInt`的第二个参数为10，即默认是十进制转十进制。
* parseFloat()方法：用于将一个字符串转为浮点数。此方法会自动过滤字符串前导的空格，如果字符串包含不能转为浮点数的字符，则不再进行往后转换，返回已经转好的部分。如果参数不是字符串，或者字符串的第一个字符不能转化为浮点数，则返回`NaN`。

2、String类型

由于历史原因，JavaScript引擎不能自动识别编号大于0xFFFF的Unicode字符，JS允许在程序中使用Unicode编号表示字符，写成\uxxxx的形式，例如'\u00A9' 表示 "©"。

在一些情景中，常用到转义符`\`来表示特殊符号。在表示多行字符串时，建议这样使用：

```js
var s1 = '1234'+
    '5678'
//s1 === '1234568'
ES6下：
var s2 = `1234
5678`
//"1234↵5678" 此处有一个回车符不能省略
```

3、Null类型和Undefined类型

两者都可以表示为“没有”，使用可区分于：有一个对象obj，但现在不想给值，用null；有一个非对象，不想给值，用undefined。

4、Boolean类型

布尔值代表“真”和“假”两个状态，真用`ture`表示，假用`false`表示。布尔值只有这两个值。

下列运算符会返回布尔值：

- 前置逻辑运算符： `!` (Not)
- 相等运算符：`===`，`!==`，`==`，`!=`
- 比较运算符：`>`，`>=`，`<`，`<=`

JS中只有以下六个值会被转为`false`：`undefined`;`null`;`false`;`0`;`NaN`;` ""`或`''`(空字符串)。

5、Object类型

简单说，对象就是一组“键值对”（key-value）的集合，是一种无序的复合数据集合。对象是一个复杂类型，由简单类型组成。

创建一个对象的方法可以有

```js
1.  var o1 = {}; 
2.  var o2 = new Object(); 
3.  var o3 = Object.create(Object.prototype);
```

如果想知道一个变量a是否已经声明过，可以用`if('a'  in window){...}`检测。

对象的属性的删除方法`delete`，例如：

```js
var p = { name : 'Jack'}

p.name === p['name']  //取value值

p['name'] = undefined 
'name' in p        // true，只是把name值变成了变成了undefined，但name这个key仍然存在

delete p['name']   //同时删除key-value；
'name' in p        // false
```

`for...in`循环是用来遍历对象，会跳过不可遍历的 key，不仅遍历对象自身的属性，还遍历继承的属性。

一般情况下，都是只想遍历对象自身的属性，所以使用`for...in`的时候，应该结合使用`hasOwnProperty`方法，在循环内部判断一下，某个属性是否为对象自身的属性。

```js
for (var key in person) {
  if (person.hasOwnProperty(key)) {
    console.log(key);
  }
```



参考文献：

1.[阮一峰JavaScript教程](https://wangdoc.com/javascript/types/index.html)

