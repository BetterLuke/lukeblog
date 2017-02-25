---
layout:     post
title:      "重温那些HTML与CSS的基础知识"
subtitle:   "Review the basic knowledge of HTML and CSS."
date:       2016-10-28
author:     "luke"    
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - Front-End
    - Note
---

# HTML标签

__1.__ `<head>`标签用于定义文档的头部，它是所有头部元素的容器。文档的头部描述了文档的各种属性和信息，包括文档的标题等。

__2.__ `<html></html>` 称为根标签，所有的网页标签都在`<html></html>`中。

__3.__  在`<body>`和`</body>`标签之间的内容是网页的主要内容，如`<h1>`、`<p>`、`<a>`、`<img>`等网页内容标签，在这里的标签中的内容会在浏览器中显示出来。

__4.__  __语义化：__ 说的通俗点就是,明白每个标签的用途（在什么情况下使用此标签合理）。

__5.__  `hx`标题标签：一共有6个，`h1、h2、h3、h4、h5、h6`分别为一级标题、二级标题、三级标题、四级标题、五级标题、六级标题。并且依据重要性递减。`<h1>`是最高的等级。

__6.__  `<strong>和<em>`强调文本显示。

```html
<em>需要斜体强调的文本</em>
<strong>需要加粗强调的文本</strong>
```

__7.__ `<span>`标签是没有语义的，它的作用就是为了设置单独的样式用的。

__8.__ `<q>`标签，短文本引用。注意要引用的文本不用加双引号，浏览器会对q标签自动添加双引号。语义：引用别人的话。

__9.__ `<blockquote>`标签，长文本引用。

__10.__ `&nbsp;` :空格.html代码中输入空格、回车都是没有作用的。要想输入空格，必须写入 `&nbsp;`

__11.__ `<hr/>`标签，添加水平横线。

__12.__ `<code>`适合加入单行代码；`<pre>`适合添加多行代码

__13.__ ul-li是没有前后顺序的信息列表。

```html
<ul>
  <li>信息</li>
  <li>信息</li>
   ......
</ul>
```

__14.__ ol-li是有序列表，默认从1开始。

__15__ `<dl>` 标签定义了定义列表（definition list）。

```html
<dl>
   <dt>计算机</dt>
   <dd>用来计算的仪器 ... ...</dd>
   <dt>显示器</dt>
   <dd>以视觉方式显示信息的装置 ... ...</dd>
</dl>
```

![](/img/in-post/post-note-code/Front-End/go_over_html&css/c-03.png)

__16.__ `<div>`标签的作用就相当于一个容器

__17.__ __HTML中的表格__

* `<tbody>…</tbody>`：当表格内容非常多时，表格会下载一点显示一点，但如果加上<tbody>标签后，这个表格就要等表格内容全部下载完才会显示。
* 带有 thead、tbody 以及 tfoot 元素的 HTML 表格：

```html
<table border="1">
  <thead>
    <tr>
      <th>Month</th>
      <th>Savings</th>
    </tr>
  </thead>

  <tfoot>
    <tr>
      <td>Sum</td>
      <td>$180</td>
    </tr>
  </tfoot>

  <tbody>
    <tr>
      <td>January</td>
      <td>$100</td>
    </tr>
    <tr>
      <td>February</td>
      <td>$80</td>
    </tr>
  </tbody>
</table>
```
如果使用 thead、tfoot 以及 tbody 元素，就必须使用全部的元素。它们的出现次序是：thead、tfoot、tbody，这样浏览器就可以在收到所有数据前呈现页脚了。您必须在 table 元素内部使用这些标签。

* 表格还是需要添加一些标签进行优化，可以添加标题和摘要：

```html
<table summary="表格简介文本">
    <caption>表格的标题</caption>
    ...
</table>
```
摘要的内容是不会在浏览器中显示出来的。它的作用是增加表格的可读性(语义化)，使搜索引擎更好的读懂表格内容，还可以使屏幕阅读器更好的帮助特殊用户读取表格内容。

__18.__ `<a>`标签，链接到另一个页面

* `<a>`标签在默认情况下，链接的网页是在当前浏览器窗口中打开，有时我们需要在新的浏览器窗口中打开。如下代码：

```html
<a href="目标网址" target="_blank"></a>
```
* `<a>`标签可以链接Email地址，使用mailto能让访问者便捷的向网页管理者发送电子邮件，图示：
![](/img/in-post/post-note-code/Front-End/go_over_html&css/c-06.jpg)

__19.__ 认识`<img>`标签

语法：

```html
<img src="图片的地址"  alt="下载失败时的替换文本" title="提示文本">

src: 标示图片的位置；
alt：指定图像的描述性文本，当图像不可见时（下载不成功时），可看到该属性指定的文本；
title：提供在图像可见时对图像的描述(鼠标滑过图片时显示的文本)；
```

__20.__ HTML中的表单

* 认识表单：把用户在浏览器里输入的数据传送到服务区端。

```html
<form method="传送方式" action="服务器文件">...</form>
```

* 文本域，多行文本输入：

```html
<textarea rows="行数" cols="列数">文本</textarea>
```

* 单选框与多选框：

```html
<input  type="radio/checkbox"  value="值"  name="名称" checked="checked"></input>
```

1. type: 当 type="radio" 时，控件为单选框; <br>当 type="checkbox" 时，控件为复选框。
2. value：提交数据到服务器的值（后台程序PHP使用）
3. name：为控件命名，以备后台程序 ASP、PHP 使用
4. checked：当设置 checked="checked" 时，该选项被默认选中

同一组的单选按钮，name 取值一定要一致，比如上面例子为同一个名称“radioLove”，这样同一组的单选按钮才可以起到单选的作用。

* `<select>`下拉列表框:

```html
<label>爱好</label>
<select>
      <option value="看书">看书</option>
      <option value="旅游" selected='selected'>旅游</option>
      <option value="运动">运动</option>
      <option value="购物">购物</option>
</select>
```

![](/img/in-post/post-note-code/Front-End/go_over_html&css/c-02.gif)

同样，下拉列表框同样支持多选，`<select>`标签中设置`multiple="multiple"`属性，就可以实现多选功能，在widows操作系统下，进行多选时按下Ctrl键同时进行单击（在 Mac下使用 Command +单击）。

```html
<label>爱好</label>
<select multiple="multiple">
  <option value="看书">看书</option>
  <option value="旅游" selected="selected">旅游</option>
  <option value="运动">运动</option>
  <option value="购物">购物</option>
</select>
```

![](/img/in-post/post-note-code/Front-End/go_over_html&css/c-01.jpg)

* 提交按钮：

```html
<input type="submit" value="" name=""></input>
```

演示：

```html
<form method="post" action="save.php">
  <label for="myName">姓名：</label>
  <input type="text" value="" name="myName">
  <input type="submit" value="提交" name="submitBtn">
</form>
```

![](http://img.mukewang.com/52e6126f0001496a04480218.jpg)

* 重置按钮：

```html
<input type="reset" value="" name="">
```

演示：

```html
<form method="post" action="save.php">
  <label for="myName">姓名：</label>
  <input type="text" value="" name="myName">
  <input type="reset" value="重置" name="resetBtn">
</form>
```

![](http://img.mukewang.com/52e6189e000186b604480218.jpg)
![](http://img.mukewang.com/52e618bc00015a1004480218.jpg)

* __form表单中的label标签__ <br>
label标签不会向用户呈现任何特殊效果，它的作用是为鼠标用户改进了可用性。如果你在 label 标签内点击文本，就会触发此控件。就是说，当用户单击选中该label标签时，浏览器就会自动将焦点转到和标签相关的表单控件上（就自动选中和该label标签相关连的表单控件上）。

__21.__ `<cite>`标签标签定义作品（比如书籍、歌曲、电影、电视节目、绘画、雕塑等等）的标题。默认效果是被包裹的文字斜体。重要的是语义化。

## CSS篇

#####  1.代码注释：

```html
/*注释语句*/
```

##### 2. 优先级：`内联式 > 嵌入式 > 外部式`

##### 3.  CSS权值：

标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100，还有一个权值比较特殊--继承也有权值但很低，有的文献提出它只有0.1，所以可以理解为继承的权值最低。

```html
p{color:red;} /*权值为1*/
p span{color:green;} /*权值为1+1=2*/
.warning{color:white;} /*权值为10*/
p span.warning{color:purple;} /*权值为1+1+10=12*/
#footer .note p{color:yellow;} /*权值为100+10+1=111*/
```

可以使用!important设置最高权值，这样发生重叠时。被声明为!important的css样式不会被覆盖：

```html
p{color:red!important;}
p{color:green;}
<!-- 字体颜色是红色 -->
```

##### 4. 字体排版：

```
font-family:“字体的名称”      ==> 设置字体
font-weight:bold;            ==> 设置为粗体
font-style:italic;           ==> 设置为斜体
font-size: __px              ==> 设置字体大小
color:                       ==> 设置字体颜色
text-decoration:underline    ==> 下划线
text-decoration:line-through ==> 删除线
```

__注意：__ 在缩写时 font-size 与 line-height 中间要加入“/”斜扛，如：

```css
body{
    font:12px/1.5em  "宋体",sans-serif;
}
```

##### 5. 段落排版

```
test-indent:2em              ==> 文段缩进两个文字
line-height: __em            ==> 行间距
letter-spacing:__px          ==> 文字间隔或者字母间隔
word-spacing:__px            ==> 英文单词之间的间距
text-align:center/right/left ==> 设置块状元素内的文本居中方向
```

##### 6. 元素分类

![](/img/in-post/post-note-code/Front-End/go_over_html&css/c-04.png)

默认为内联元素的元素，在CSS中可以显示的声明为块状元素或内联块状元素：

```css
a{display:block;}; /* 声明设置内联元素a为块状元素 */
a{display:inline-blaock;} /* 声明设置内联元素a为内联块状元素 */
```

当然块状元素也可以显式的声明为内联元素，享受内联元素的特点：

```css
div{display:inline;} /*生命设置块状元素为内联元素 */
```

##### 7. 盒式模型

一个盒模型元素的实际宽度：

![](http://img.mukewang.com/539fbb3a0001304305570259.jpg)

css内定义的宽（width）和高（height），指的是填充以里的内容范围。就比如：

```css
div{
    width:200px;
    padding:20px;
    border:1px solid red;
    margin:10px;    
}
```

![](http://img.mukewang.com/543b4cae0001b34304300350.jpg)

元素的实际长度为：10px+1px+20px+200px+20px+1px+10px=262px。

##### 8. css布局模型

![](/img/in-post/post-note-code/Front-End/go_over_html&css/c-05.png)

###### Relative与Absolute组合使用:

(1)、参照定位的元素必须是相对定位元素的前辈元素：

```html
<div id="box1"><!--参照定位的元素-->
    <div id="box2">相对参照元素进行定位</div><!--相对定位元素-->
</div>
```

(2)、参照定位的元素必须加入position:relative;

```css
#box1{
    width:200px;
    height:200px;
    position:relative;        
}
```

(3)、 定位元素加入position:absolute，便可以使用top、bottom、left、right来进行偏移定位了。

```css
#box2{
    position:absolute;
    top:20px;
    left:30px;         
}
```

这样box2就可以相对于父元素box1定位了（这里注意参照物就可以不是浏览器了，而可以自由设置了）。

##### 9.颜色值

（1） 普通颜色用英文名表示。

```css
p{color:red;}
```

（2） RGB颜色：每一项的值可以是 0~255 之间的整数，也可以是 0%~100% 的百分数。

```css
p{color:rgb(133,45,200);}
p{color:rgb(20%,33%,25%);}
```

（3） 这种颜色设置方法是现在比较普遍使用的方法，其原理其实也是 RGB 设置，但是其每一项的值由 0-255 变成了十六进制 00-ff。

```css
p{color:#00ffff;}
```
