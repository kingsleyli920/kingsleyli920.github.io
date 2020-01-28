---
title: es6_summary_1
date: 2020-01-27 19:05:38
tags: 
 - JavaScript
 - ECMAScript 6
 - Front End Development
 - Web Development
comments: true
categories: Front-end
img: https://blog-imgs-1253907084.cos.ap-beijing.myqcloud.com/blog-cover-imgs/frontend-js-article1.jpeg
---
<center> <h1>ES6 Summary #1</h1> </center>
<center> <h2> <font color = lightgray>Let, Const and Var</font> </h2> </center>

<font size = 3>

## Preface

I have learned and got in touch with Front End Development for several years. Most experiences from university and internship. But I don't have too many industrial level experiences. Speaking of Front End Development, I should talk about the history of Front End. From the simplest UI-like work, till now, various Front End Frameworks have emerged in endlessly, and Front End Development has also changed from insignificant to become as important as back-end now in web development. Also due to the development of Web Front End technology in recent years, IOS and Android technologies have been squeezed and gradually been merged into the Front End Development in some company. This makes the importance of Front End of Web development faintly surpasses the importance of Back End Development.

The above is my personal understanding of the changes in the Front End. Excluding Android and IOS, Front End technology, no matter what framework or library you use, JavaScript is always the foundation and the most important part. JavaScript is mainly composed of ECMAScript (core), DOM (Document Object Model) and BOM (Browser Object Model).

ES6 (ECMAScript 6) was launched in 2015, and it has been launched for a few years. With the development in recent years, many new features have been added. Although there are a few versions later than ES6, like ES2017 and ES2019, it seems that the penetration rate is not as high as ES6, and the compatibility is even less. Having said that, ES6 guarantees a certain degree of backward compatibility. I will summarize some new basic knowledge points of ES6 later, including new keywords, new concepts, etc.

## Compatibility of ES6

First let's talk about the compatibility of ES6. ES6 supports most of the latest versions of browsers, including Edge, Firefox 68+, Chrome 78+, Opera65 +, Safari 12.1+ and other desktop browsers. The support rate is almost 100%. Mobile browsers, including IOS12 +, Samsung 9+, OperaMobile 54+ are also close to 100% compatible. As for IE, it is basically not supported, but you can convert ES6 code to ES5 code.

### Compatibility of ES6 with IE (and other browsers)

1. Compatible with basic syntax (excluding promises and some classes)

Before importing other scripts, import browser.min.js and set the type of the script tag to text / babel

2. If using new features like Promise

Babel compiles and converts JavaScript statements by default, and cannot convert new APIs. But Polyfill technology can be used. Import browser-polyfill to the development page:

	<script type="text/javascript" src="你的browser-polyfill路径"></script>

## The use of "let", "const" and the difference with "var"

Let and const are new keywords in ES6. They have the same effect as var and are used to declare variables. But there are also many differences between them. If you need to consider more compatibility, it is better to use var. Let's talk about their uses and differences.

### let's talk about "var" first:

- scope of "var" is global or entire function block
- varibles defined by "var" can be pre-parsed, that is, variables can be used before definition, such as:

	`console.log(a); var a = 10;`
- varibles defined by "var" can be redefined:

	`var a = 10; console.log(a); var a = 20; console.log(20);` result is 10 20

### let

- Variables or statements and expressions declared by "let", the scope of "let" is block-level scope (simple and rough can be understood as a block-level scope surrounded by two braces)
- In the same scope, "let" cannot be repeatedly declared, for example:：

	`let a = 10; console.log(a); let a = 20;` 
	
	The above code will report an error because the "a" variable is repeatedly declared in the same scope.
- let cannot be pre-parsed, that is:

	`console.log(a); let a = 10;` <font color="red">will cause an error</font>
	
### const

const is simple, it has all the features of let, and the variables defined are constants and cannot be repeatedly assigned.

## Summary

I have introduced the basics of "let", "const", and "var" above. It is very simple. The emergence of "let" and "const" has optimized the declaration of variables, expressions, and so on. If you don't consider compatibility, let's use "let" instead of "var".
	
## References

+ https://www.jianshu.com/p/13444c467ce2
+ https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla

</font>