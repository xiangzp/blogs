---
title: 读JS红皮书-第一章 简介
date: 
        2020-01-16 15:29:18
tags: [JavaScript高级程序设计,读书笔记]
categories:
- 前端
- JavaScript高级程序设计

top_img: https://cdn.pixabay.com/photo/2020/01/13/13/59/landscape-4762468_1280.jpg
---

# JS 简史

一般情况下，小说都从主角的背景开始说起。按照惯例，我们也来介绍介绍JS。

* 1995年2月，布兰登·艾奇为网景公司的浏览器 Navigator2 开发脚本语言`LiveScript` 。在服务器端，叫做 `LiveWire`。后发布前期，为了蹭Java的热度，改名为 `JavaScript`

  > 很多人认为NodeJs运行在服务器端是一个伟大创举，然而JS在设计之初，就是可以运行在服务器端和浏览器端的。

* Javascript 1.0发布后大获成功，后发布Javascript1.1。同时IE3 也发布了`JScript`（js相同实现）

* 1997年，以 JavaScript1.1 为原型的蓝本被提交给 `ECMA`（欧洲计算机制造商协会）。是不是很眼熟，我们使用的ECMA 5/6/7 ，都是这么命名的。协会指定39号委员会负责“ 标 准 化 一 种 通 用 、 跨 平 台 、 供 应 商 中 立 的 脚 本 语 言 的 语 法 和 语 义 ” 。这就是`TC39`。

* 数月后，TC39完成了 `ECMA-262` 标准。自此JS有了有了自己的标准，也有了新名字 `ECMAScript`。

## JS的组成

* 核心（ECMAScript）
* 文档对象模型 （DOM）
* 浏览器对象模型 (BOM)

### ECMAScript

js的核心。然而Web浏览器只是ECMAScript的可能*宿主环境*之一。比如其他的实现包括Node和Adobe Flash。

ECMAScript的组成：

* 语法
* 类型
* 语句
* 关键字
* 保留字
* 操作符
* 对象

ECMAScript的版本：

* 第1版。也就是最初发行的Javascript 1.1，删除了所有针对浏览器的代码并作了一些较小的改动

* 第2版只做了些许编辑加工

* 第3版才是对标准第一次真正的修改，标志着ECMAScript成为一门真正的编程语言。改动的内容包括：
  
  * 字符串处理。新增了的支持。
  * 错误定义。新增了
  * 数值输出。
  * 新增了 *正则表达式* 、*`try-catch`异常处理*、*新控制语句* 的支持。

* 第4版做了全面修订。提出了强类型变量，新语句和新数据结构，真正的类和经典继承。可以说是ES6/7的原型。然后因为跨度太大而夭折。

* 第5版是第4版提出时的替代建议，本身是ES3.1，然后更正为ES5。并增加了

  * 原始JSON对象
  * 继承的方法
  * 高级属性定义： `defineProperty`, `getOwnPropertyDescriptor`等
  * 严格模式

### 文档对象模型-DOM

文档对象模型（ DOM， Document Object Model）是针对 XML 但经过扩展用于 HTML 的应用程序编程接口（ API， Application Programming Interface）。

#### DOM级别

> DOM不是只有JavaScript，也有别的语言实现了DOM。比如SVG，MathML,SMIL

* DOM0级。理论上不存在，只是IE4.0和Navigator4.0最初支持的DHTML。
* DOM1级。1998年成为W3C的推荐标准。由两个模块组成：
  * DOM 核心： 规定如何映射基于XML的文档结构，简化文档操作
  * DOM HTML： 在核心基础上扩展了针对Html的对象方法
* DOM2级。扩充了鼠标和用户界面事件、范围、遍历等细分模块，通过对象接口增加了对CSS的支持。核心模块也开始支持XML命名空间。
  * DOM views
  * DOM Events ： 事件处理
  * DOM Style： CSS支持
  * DOM遍历和范围


### 浏览器对象模型-BOM
根本上来说，BOM只处理浏览器窗口和框架：
  * 弹出新浏览器窗口
  * 移动、缩放和关闭浏览器窗口
  * 提供`navigator`对象，获取浏览器的详细信息
  * 提供`location`对象，获取页面的详细信息
  * 提供`screen`对象，获取显示器分辨率信息
  * 支持 `cookies`
  * 支持 `XMLHttpRequest`和IE的`ActiveXObject`。也就是XHR。

  
