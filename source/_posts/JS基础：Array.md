---
title: JS基础：Array
date: 2019-03-01 23:46:49
tags:
---

数组是最简单的内存数据结构。JavaScript里也有数组类型，用于存储一系列同一种数据类型的值。

## 数组与伪数组

伪数组(`arrLike`)，是一个类似数组的对象。例如

``` javascript
obj = {
  0:a,
  1:b,
  2:c,
  length:3
}
```

function的`arguments`对象，还有`getElementsByTagName`、`ele.childNodes`等返回的`NodeList`对象，或者自定义的某些对象，这些都可以是伪数组。 
数组跟数组一样按索引方式存储，具有`length`属性。但是伪数组与数组的最重要区别在于，伪数组的原型链中**不包括**`Array.prototype`。

伪数组转换成数组的方法有：

* 遍历伪数组并加入一个空数组

  ```javascript
  var arr =[]
  for(var i =0;i< arrLike.length; i++){
      arr.push(arrLike[i])
  }
  ```

* 数组的`slice()`方法

  `Array.prototype.slice.call(arrLike)`

  ```js
  function list() {
    return Array.prototype.slice.call(arguments);
  }
  
  var list1 = list(1, 2, 3); // [1, 2, 3]
  ```

* ES6中的`Array.from()`方法

  ```js
  Array.from({  
    0:"kk",  
    1:18,  
    2:"文字文字", 
    length:3  });  
  //["lk", 12,"文字文字"] 
  ```

## 数组属性

数组属性有`Array.length`；`get Array[]`；`Array.prototype`

## 数组方法

数组的操作方法比较多，详细可以查阅[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array#Properties)。这里简要介绍几种常用的操作方法

举例`var num = [1,2,3,4,5]`

#### 增删查的方法

查：可以通过索引访问数组元素，如`num[1] //2`；

增：`push`方法添加一个或者多个元素到数组末尾，如`num.push[7,8]`；

​        `unshift`方法将数值直接插入数组的首位，如`num.unshift[-2,-1]` 。

删： 删除数组最靠后的元素`pop()`；删除数组首位元素`shift()`；

另外，`splice()`方法可以实现在任意位置添加或删除元素，例如

```js
num.splice(2,2) 
//第一个参数表示要删除元素的索引值，第二个参数表示删除元素的个数
//num由[1,2,3,4,5]变成[1,2,5]

num.splice(2,0,7,8,9)
//添加元素，所以第二个参数为了0；第三个元素以后，表示要添加到数组里的值
//num由[1,2,3,4,5]变成[1,2,7,8,9,3,4,5]
```

#### Array.prototype.forEach

`forEach() `方法对数组的每个元素执行一次提供的函数，用法`array.forEach(function(value,key){})`。需注意的是`forEach()`不会在迭代之前创建数组的副本，已删除的项不会被遍历到。如果已访问的元素在迭代时被删除了（例如使用 `shift()`），之后的元素将被跳过。

```js
function forEach(array,x){
    for (let i =0;i<array.length;i++){
        x(array[i],i)
    }}
forEach(['a','b','c'],function(value,key){
    console.log(value,key)})

//a 0
//b 1
//c 2
```

####Array.prototype.sort()

`sort() `方法用原地算法对数组的元素进行排序，并返回数组。

`array.sort(function(x,y){return x - y})`

`array.sort(function(x,y){return y -x})`

```js
var a = [5,6,3,1,4,2]
var b =a.sort()//b [1,2,3,4,5,6]
//a仍为[5,6,3,1,4,2]
```

#### .join() .concat() .map() .filter() .reduce()

`join() `方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。

```js
a = [1,2,'a']
a.join('cc') // '1cc2cca'
a.join()     //'1,2,a'
```

`concat()`方法用于合并两个或多个数组。此方法不会更改现有数组，而是返回一个新数组。

```js
a = [1,2,3];b=[4,5,6]
var c = a.concat(b) // [1,2,3,4,5,6]
var d = a.concat([]) // d为新数组[1,2,3]与a不同
```

`map()`方法用于遍历数组，操作并收集返回新的数组

```js
a = [1,2,3]
a.map(value => value*2) //[2,4,6],数组a不变
```

`filter() `方法创建一个新数组, 其包含通过所提供函数实现的规则的所有元素。

```js
a = [1,2,3,4,5,6,7,8,9]
a.filter(function(value,key){
    return value >= 5
}) //[5,6,7,8,9]
```

`reduce() `方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。

```js
a.reduce(functiong(sum,n){
         return sum + n
         },0)
```

```js
var a =[1,2,3]
a.reduce(function(arr,n){
    arr.push(n * 2)
    return arr
},[])
// [2,4,6]
//map()用redcue()表示
```



``` js
//计算所有奇数和
var a = [1,2,3,4,5,6,7,8,9]
a.reduce(function(arr,n){
    if(n % 2 === 1){
        return arr + n}
    return arr
},0)                         
//25
```

## 参考文献：

[MDN—JavaScript的标准库Array](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)

