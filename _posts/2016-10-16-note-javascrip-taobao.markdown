---
layout:     post
title:      "模仿淘宝前端手记之购物车"
subtitle:   "Imitation of taobao's front page about the shopping cart. As you see, this is a long note."
date:       2016-10-16
author:     "luke"    
header-img: "img/post-bg-alibaba.jpg"
catalog: true
tags:
    - JavaScript
    - Note
---

# 模仿淘宝购物车手记

## 整体思路框架

![](\img\in-post\post-note-code\JavaScript\mindmap-structure01.png)

---

## 学新知识

### 取表格的所有行元素

```javascript
var tr = cartTable.children[1].rows;
```

`rows`表格元素特有的属性，存放结点所有的tr元素。


### JavaScript中`==`和`===`的区别
```
== 等于
=== 严格等于
```

### `js_string类型`字符串里使用单引号的规范
若想在函数里传参，字符串类型是`js_string`的话,单引号内用双引号，双引号内用单引号。

如：
```javascript
setTimeout("document.getElementById('rockImg').src = 'rock.png';", 5 * 60 * 1000);
```
如果想双引号里使用双引号，注意使用转义符号转义。

###事件代理

对于动态产生的元素节点，想要绑定事件，需要使用事件代理机制。因为在该元素节点还没有产生之前，是无法通过`getElementById()`方法获取的。

![demo](\img\in-post\post-note-code\JavaScript\c-01.gif)

事件代理机制的使用方式，其实就是获取父元素的`srcElement`属性，通过判断返回对象的`className()`，筛选出想要操作的对象，然后对其操作。

像本案例中：鼠标放在已选商品的图片上，会出现“取消选择”按钮，而这个按钮，是使用DOM方法动态产生的，想要绑定它的`onclick()`事件，就需要使用使用事件代理机制：

* 获取父元素
* 定义父元素的`onclick`事件，传递__事件对象__
* 从传递的事件对象中获取`srcElement`
* 使用`switch...case...`语句分别操作

> Event对象(事件对象)代表事件的状态信息，比如事件在其中发生的元素、键盘按键的状态、鼠标的位置、鼠标按钮的状态。

```javascript
 trs[r].onclick = function(e) {
            e = e || window.event;
            var el = e.srcElement;
            var cls = el.className;
             switch (cls) {
                case 1;
                case 2;
             }
         }
```

### 删除行操作

删除购物车里的商品，就是删除所在行，可以使用`removeChild()`方法，也就是说，该行找到父元素，并调用父元素的`removeChild()`去删除它。

```javascript
    var conf = confirm("确定要删除吗？"); //confirm函数返回true或false
    if(conf) {
    this.parentNode.removeChild(this);
    }
```

![demo](\img\in-post\post-note-code\JavaScript\c-02.gif)

---

## 不懂就记

![](\img\in-post\post-note-code\JavaScript\mindmap-structure02.png)

## 心得体会

* 注意封装方法，不然一大堆代码难看到爆
* 感谢入门书《DOM编程艺术》让自己学到了一些短小精悍的方法。
* 慕课网上这位老师的编程风格很菜
* 截取屏幕gif的神器：Recodit。

## 链接

* [慕课网教学视频](http://www.imooc.com/learn/34)
* [本DEMO的github地址](https://github.com/BetterLuke/ShoppingCart-DEMO)
