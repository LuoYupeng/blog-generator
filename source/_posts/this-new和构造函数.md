

## this的指向

### 函数的call,apply.bind区别

1. call()：调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。
   `fun.call(thisArg, arg1, arg2, ...)`

2. apply()：调用一个具有给定this值的函数，以及作为一个数组（或类似数组对象）提供的参数。
   `fun.apply(thisArg, [argsArray])`

   **call()方法的作用和 apply() 方法类似，区别就是call()方法接受的是参数列表，而apply()方法接受的是一个参数数组。**

3. bind()：（创建）返回一个新函数，并且是函数内部this为传入的第一个参数。
   `fun.bind(thisArg[, arg1[, arg2[, ...]]]);`
   **bind是返回对应的函数，便于稍后调用。apply、call 则是立即调用。**

### 实际中使用函数的this

```js
btn.addEventListener('click' ,function handler(){
  console.log(this) // 请问这里的 this 是什么
})

$ul.on('click', 'li' , function(){
  console.log(this) // 请问这里的 this 又是什么
})
```

在上面两例中，我们要弄清楚`this`是什么，应该去看源码，或者看文档，千万不要瞎猜，想当然的认为this指向就是谁。

> 当一个函数被调用时，拥有它的object会作为this传入。在global下，就是window or global，其他时候就是相应的object。
>
> 出现在函数嵌套里时，函数里面嵌套的函数是属于哪个object的，this就是哪个object，如果没有就是window。

摒弃语法糖，func()转化为func.call(undefined,arg1,arg2)

> func(p1, p2) 等价于
> func.call(undefined, p1, p2)
>
> obj.child.method(p1, p2) 等价于
> obj.child.method.call(obj.child, p1, p2)

箭头函数中

> **箭头函数只能用赋值式写法，不能用声明式写法**
>
> **默认绑定外层this，不能用call方法修改里面的this。**

举例：

```js
function X(){
  return object = {
    name: 'object',
    options: bull,
    f1(x){
      this.options = x
      this.f2()
    },
    f2(){
      this.options.f2.call(this)
    }
  }
}
var options = {
  name: 'options',
  f1(){},
  f2(){
    console.log(this)  //this是啥？//结果是object
  }
}

var x = X()
x.f1(options)

//由于X()就是object，x.f1(options)相当于object.f1.call(object,options)
//进入到f1(x){}这里，这里的x是参数也就是options，f1(options){}里的this是object
//this.f2()→object.f2();this.options =x 也就是options
//相当于options.f2.call(object)
```



### new和构造函数

以游戏中，大批量创造小兵为例。

```js
var 小兵 = {
  ID: 1, //区别每个士兵
  power: 5, //攻击力
  life: 50, //生命值
  run: function(){/*奔跑的代码*/}
  attack: function(){/*攻击的代码*/}
  died: functong(){/*死亡的代码*/}
}
var 小兵2 = { ... }
var 小兵3 = { ... }
兵营.制造(小兵)
兵营.制造(小兵2)
兵营.制造(小兵3)
```

当需要大批量成千上百制造小兵时，可以用一个for循环。

```js
var 小兵们 = []
var 小兵
for(var i=0;i<100;i++){
  小兵 = {
    ID: i,
    /* 其余奔跑，攻击，死亡等代码 */
  }
  小兵们.push(小兵)
}
兵营.批量制造(小兵们)
```

但使用For循环时，每一个士兵中的奔跑代码，攻击代码等是一模一样，但是都会占用内存，这样就会使占用内存非常大。

引入原型链改进：

```js
var 小兵共有属性 = {
  兵种: '无名小卒',
  power: 5, //攻击力
  run: function(){/*奔跑的代码*/}
  attack: function(){/*攻击的代码*/}
  died: functong(){/*死亡的代码*/}
}
var 小兵们= []
var 小兵
for(var i=0;i<100;i++){
  小兵 = {
    ID: i,
    life: 50
  }
  小兵.__proto__ =小兵共有属性
  小兵们.push(小兵)
}
兵营.批量制造(小兵们)
```

再优化：

```js
//制造小兵
function create小兵(id){
  var 小兵 = {
    ID: i,
    life: 50
  }
  小兵.__proto__ = create小兵.小兵共有属性
  return 小兵
}
create小兵.小兵共有属性 = {
  兵种: '无名小卒',
  power: 5, //攻击力
  run: function(){/*奔跑的代码*/}
  attack: function(){/*攻击的代码*/}
  died: functong(){/*死亡的代码*/}
}

//使用生产小兵，使用与制造分开
var 小兵们 = []
for(var i=0;i<100;i++){
  小兵们.push(create小兵(i))
}
兵营.批量制造(小兵们)
```

#### new的出现：

![new.jpg](https://i.loli.net/2019/04/06/5ca78d66ec25f.jpg)

```js
//制造小兵
function 小兵(id){
  //var temp = {}  //new的作用省略了4步注释的代码 //1
  //this = temp                            //2
  //this.__proto__ = create小兵.prototype   //3
  //create小兵.prototype = {constructor: 小兵} //5
   this.ID = id, //自有属性
   this.生命值 = 50 //自有属性
  //return this                            //4
}
小兵.prototype = {
  //共有属性
  constructor: 小兵, //记录由谁构造(构造函数)，直接对prototype赋值会覆盖掉上面第5步内容需要重新
  兵种: '无名小卒',                          //需要重新对constructor赋值
  power: 5, //攻击力
  run: function(){/*奔跑的代码*/}
  attack: function(){/*攻击的代码*/}
  died: functong(){/*死亡的代码*/}
}

//使用生产小兵，使用与制造分开
var 小兵们 = []
for(var i=0;i<100;i++){
  小兵们.push( new 小兵(i))
}
兵营.批量制造(小兵们)
```

直接重写prototype是比较危险的做法，建议的用法是将共有属性添加进prototype。

```js
小兵.prototype.兵种 = '无名小卒'
小兵.prototype.power = 5
//依次类推
```

**new小结**

```js
var object = new Object()
//自有属性空
//object.__proto__ === Object.prototype

var array = new Array('a','b','c')
//自有属性0:'a',1:'b',2:'c',length: 3
//array.__proto__ === Array.prototype
//Array.prototype.__protot__ === Object.prototype

var fn = new Function('x','y','renturn x+y')
//自有属性 length:2,不可见函数体:'return x+y'
//fn.__proto__ === Function.prototype

Array is  a function
Array = function(){...}
Array.__proto__ === Function.prototype
```





## 参考引用：

[JavaScript 中的 this](<https://zhuanlan.zhihu.com/p/24107744>)

[this 的值到底是什么？一次说清楚](https://zhuanlan.zhihu.com/p/23804247)

[JS 的 new 到底是干什么的？](<https://zhuanlan.zhihu.com/p/23987456>)