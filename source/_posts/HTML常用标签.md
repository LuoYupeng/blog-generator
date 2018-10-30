---
title: HTML几个标签
date: 2018-10-30 22:13:45
tags: HTML
description: 

---

这里介绍几个HTML常用的标签：`<iframe> <a> <form> <input>/<button> <table>`。
#  `<iframe>`标签
`<iframe>` 标签规定一个内联框架。一个内联框架被用来在当前 HTML 文档中嵌入另一个文档。常用写法：` <iframe src = "https://xxx.com" name= "xxx"> </iframe> `
常用属性有：
`name`iframe名称，可以用作`<a>`标签，`<form>`标签的target属性值，或`<input> `标签和 `<button>`标签的formtarget属性值；
`height` `width` 规定高度和宽度；
`frameborder = 1/0` 规定是否显示iframe周围边框；
`sandbox`该属性对呈现在iframe框架中的内容启用一些额外的限制条件；
目前iframe标签常用在邮箱，广告上。


  `<a>` 标签
===============
`<a>`可以创建一个到其他网页、文件、同一页面内的位置、电子邮件地址或任何其他URL的超链接。
示例：
> `<a href="xxx" target = "yyy"> zzz </a>`

`<a>`标签中属性href的值:
1. 可以是网址，也可以是相对路径； 
2. 带#前面的值，指向锚点，页面不刷新；
3. "?xxx=yyy"，发起请求
4. 伪协议，href="javascript: ;"，点击后，页面不做任何事情，用于一些特殊情景

`<a>`标签中属性target的值：`_self`默认值，在当前页面加载链接；`_blank`新窗口打开；`_parent`在父框架打开；`_top`在顶层框架打开；`framename`与iframe同时使用。
`<a>`标签中属性download定义了下载链接的地址。


`<form>`标签
====================
 表单提交标签，`<form>` 标签通常包含一个或多个如下的表单元素：
` <input>  <textarea> <button> <select> <option> <optgroup> <fieldset> <label>  `。

示例: 
> 
```html
<form action ="users" method="post"> 
    <input type ="text" name="username">
        <input type ="password" name="password">
        <input type ="submit" value="提交">
</form>
```
 
 常用属性和值的相对关系
 
| 属性 | 值 | 描述 |
| --- | :--- | :--- |
|mathod|get<br>post|规定用于发送表单数据的 HTTP 方法|
|action|url|规定当提交表单时向何处发送表单数据
|name|text|规定表单的名称

`<input>/<button>/<select>`标签
===================
input与button的不同在于：button默认为submit，提交；而input默认是输入框，需要将属性type="submit"才能提交。
实际使用通常用`<lable>`标签包裹`<input>`使用，示例：
`<label><input type='radio' name='fruit' value='banana'>香蕉</label>`
`<label><input type='radio' name='fruit' value='apple'>苹果</label>`
其中input属性type=radio时为单选框，type=checkbox时为复选框。
`<select>`默认是单选，属性multiple为true时，为多选。示例：
> 
```html
<select name="group" > 
  <option value=" "> -</option >
  <option value="1">第一组 </option >
  <option value="2" disabled>第二组</option > ##disable表示不能被选中
  <option value="3" selected>第三组</option > ##selected表示默认选中 
</select > 
```


`<table>`标签
======================
代码示例：
<details>

```html
<table >
    <colgroup>
    <col width=70>
    <col bgcolor=gray width=100>
    <col width=50>
    <col bgcolor=red width=50>
  </colgroup>
    <thead>
      <tr>
        <th>项目</th>
        <th>姓名</th>
        <th>班级</th>
        <th>分数</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th></th>
        <td>小明</td>
        <td>一班</td>
        <td>90</td>
      </tr>
      <tr>
        <th></th>
        <td>小红</td>
        <td>二班</td>
        <td>95</td>
      </tr>
      <tr>
          <th>平均分</th>
          <td></td>
          <td></td>
          <td>92.5</td>
      </tr> 
    </tbody>
    <tfoot>
        <tr>
            <th>总分</th>
            <td></td>
            <td></td>
            <td>185</td>
        </tr>
    </tfoot>
  </table>  
  ```

</details>
效果图片：
<img src="http://ph5zfz0ki.bkt.clouddn.com/18-10-31/76207176.jpg"  width="400" height="200">

参考文献：
* [iframe,我们来谈一谈](https://segmentfault.com/a/1190000004502619#articleHeader15)
* [MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)

<strong>Fighting!</strong>