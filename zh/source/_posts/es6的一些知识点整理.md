---
title: es6的一些知识点整理
date: 2020-01-02 01:13:43
tags: 
 - JavaScript
 - ECMAScript 6
 - 前端开发
 - Web开发
comments: true
categories: Front-end
coverImg: ../images/frontend-js-article1.jpeg
---
<center> <h1>es6的一些知识点整理</h1> </center>
<center> <h2> <font color = lightgray>基础部分</font> </h2> </center>

<font size = 3>
##开篇
接触前端也很久了，当然主要都是在学校，工业级的不太多，仅仅是实习中的有限的时间接触了一些。说到前端，不得不说前端的发展历程，从最简单的类似UI的工作，到现在，各种前端框架层出不穷，前端开发也从Web开发的无足轻重，变成现在和后端一样的重要。同样由于近几年Web前端技术的发展，IOS和Android技术被挤压的很厉害，也渐渐并入前端当中。使得现在前端在Web开发中占有的比重隐隐超过后端。

以上是我个人对前端变化的理解。排除Android和IOS，Web前端技术，不论你是用什么框架，什么库，JavaScript永远是基础，也是最重要的一部分。JavaScript主要由ECMAScript（核心），DOM（文档对象模型）和BOM（浏览器对象模型）。

ES6（ECMAScript 6）是2015年推出的，也推出一段时间了，随着这几年发展，添加了很多新特性，虽然后面还有ES2017，ES2019，但是貌似普及率没有ES6高，兼容性更不要说了，ES6保证了一定的向下兼容性，后面我会总结下ES6的一些新出的基础知识点，包括新增关键词，新增概念等。

##ES6的兼容性
首先先说ES6的兼容性，ES6支持绝大多数最新版本浏览器，包括Edge，Firefox 68+， Chrome 78+，Opera65+，Safari 12.1+等桌面版浏览器支持率几乎达到100%。移动版浏览器，包括IOS12+，Samsung 9+， OperaMobile 54+兼容程度也接近100%。至于IE，就几本不支持了，但是可以用代码转换。

###ES6兼容IE（和其它浏览器）的方法
1. 兼容基本语法（不包括Promise之类的）

在引入其它脚本前，引入browser.min.js， script标签的type设为text/babel

2. 如果使用Promise等新特性

Babel默认编译转换JavaScript语句，不能转换新的API。但是可以使用Polyfill（代码填充）技术。在开发页面中引入browser-polyfill即可：

	<script type="text/javascript" src="你的browser-polyfill路径"></script>
	
##let，const的使用以及它们和var的区别

let和const是ES6新出来的关键字，和var作用相同，用来声明变量。但是也有很多不同。如果需要考虑兼容性更多，还是推荐用var比较好。接下来说下它们的使用和区别。

###说let和const之前，先说说var的一些特性
- var的作用域是全局或者整个函数块
- var可以被预解析，也就是说，可以在定义前使用变量，比如：

	`console.log(a); var a = 10;`这个是不会报错的，因为var定义的a被预解析。
- var定义的变量在同一作用域下能重复定义

	`var a = 10; console.log(a); var a = 20; console.log(20);` 结果是10 20

###let
- let声明的变量或者语句以及表达式，作用域为块级作用域（简单粗略可以理解成是两个大括号包裹着是一个块级作用域）
- 同一作用域下，let不能重复声明，比如：

	`let a = 10; console.log(a); let a = 20;` 上述代码会报错，因为在同一个作用域中重复声明了a变量。
- let不能被预解析，也就是：

	`console.log(a); let a = 10;`是会报错的。
	
###const

const 就没什么可说的了，它拥有let的所有特性，并且定义的变量为常量，不可重复赋值。

###结语

let，const和var基本介绍完了，很简单，let和const的出现，优化了声明变量，表达式等。如果不考虑兼容性，推荐使用let代替var。
	
###参考链接
+ https://www.jianshu.com/p/13444c467ce2
+ https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla

</font>