---
title: 初识HTML
date: 2018-10-29 23:46:05
tags: HTML
---
**W3C**，全称是万维网联盟 _(World Wide Web Consortium,W3C)_ ，是万维网的主要国际标准组织。W3C推荐标准的目的在于使万维网技术标准化。HTML5是目前W3C制定的HTML最新修订版本。
** [MDN](https://developer.mozilla.org/zh-CN/)** (旧称Mozilla Developer Network)，是一个汇集众多Mozilla基金会产品和网上技术开发文档的免费网站。在MDN可以帮助更好的学习HTML知识，相比于直接看HTML标准文档，MDN显得更友好一点。
例如，MDN可以直接查询HTML5中的所有有效的[标签列表](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)。
<b>空标签</b>，是指不可能存在字节点的元素,通常在一个空元素上使用一个闭合标签是无效的。在MDN上列出所有了[空元素](https://developer.mozilla.org/zh-CN/docs/Glossary/%E7%A9%BA%E5%85%83%E7%B4%A0)： 
``` 
<area> <base> <br> <col> <colgroup> <command> <embed> 
<hr> <img> <input> <keygen> <link> <meta> <param>
<source> <track> <wbr>
```
**[可替换标签](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element)：**  在CSS 里，可替换元素的展现不是由CSS来控制的。这些元素是一类外观渲染独立于CSS的外部对象。
 典型的可替换元素有 ``` <img> <object> <video> ``` 和 表单元素，如```<textarea> <input> ```。 某些元素只在一些特殊情况下表现为可替换元素，例如 ```<audio>``` 和``` <canvas>``` 。
例如,img元素的内容通常会被其`src`属性指定的图像替换掉。替换元素通常有自带宽高，但也可以通过属性覆盖自带宽高，也可以由CSS覆盖属性。