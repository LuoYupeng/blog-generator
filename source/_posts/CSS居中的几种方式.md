---
title: CSS居中的几种方式
date: 2018-11-06 23:34:14
tags:  CSS
---
CSS：层叠样式表(Cascading Style Sheets)。
CSS由于属性和值的多样复杂，一般都是使用过程中边用边查询。
引入CSS的方式有：
> * style内联属性
> * 在HTML`head`使用`<style>`标签
> * 通过`<link>`引入
> * 通过@import url(./style.css)引入

文档流
=======
一个元素的高度由其内容决定，块极元素由其文档流_(normal flow)_ 的高度总和决定。
其中文档流的含义：文档内元素流动的方向，内联元素从左到右，块级元素从上到下，每一快独占一行。
浮动_(float)_ 、绝对定位_(absolute)_ 、固定定位_(fixed)_ 三种方式定位会脱离文档流。
其中：
1、`position: fixed`固定定位是相对于窗口定位，用`top``left`定位；
2、绝对定位是相对父元素定位
> 子元素： `position: absolute`
> 父元素： `position: relative`

3、浮动，子元素使用`float: left/right`父元素添加`.clearfix::after{
      content: "";
      display: block;
      clear:both}`，用来解决可能出现的bug。

`box-sizing: border-box`
========================
`border-box`与`content-box`对比，`border-box`更接近物理世界中的 Box。比如仓库中摆放纸箱. margin 就是箱与箱间的距离，border 就是纸箱纸壳厚度。 padding 就是纸箱中用来减震的泡沫塑料厚度。
有人评论，box-sizing属性明显是给box model花式擦屁股，因为box model默认的是`content-box`，请在CSS开头声明：`*,
*::before,
*::after {
  box-sizing: border-box;
}`

水平居中
==========
CSS中，进行水平居中是相对比较容易的。
如果它是一个行内元素，就对它父元素应用： `text-align: center`;
如果它是一个块级元素，就对她自身应用`margin: auto`。

垂直居中
================
#### Flex布局居中
主流方式，广泛用于PC端和移动端
```html
.父元素 {
        display: flex;
        aligin-items: center;
        justify-content: center 
        }
``` 
#### grid布局居中
非主流，未来可能取代flex的用法，兼容性待提高
```html
.父元素{
    display: grid;
    align-items: center;
    justify-content: center;
}
```

#### 绝对定位居中
适用于使用绝对定位的场景，传统方法
```html
.父元素{
    position: relative;
}
.子元素{
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

#### 内联元素居中
对块级父级使用，能让内部的匿名行盒(文字)、行内元素(span)、inline-block元素在父亲里水平居中
```html
.container {
    text-align: center;
}
```

css使用个人小tips：
1、写样式时，由内到外，由小到大；
2、先写垂直间距，后写左右间距；
3、伪类:（一个冒号，虚，状态），伪元素::（两冒号，确实存在，伪装，可以display，不可被选中或复制；
4、使用`display：inline-block`与`vertical-align: top`同时存在使用。

<br>
<strong>Fighting!</strong>


