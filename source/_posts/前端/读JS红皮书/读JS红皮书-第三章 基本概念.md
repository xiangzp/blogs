---
title: 读JS红皮书-第三章 基本概念
date: 
        2020-01-17 10:57:34
tags: [JavaScript高级程序设计,读书笔记]
categories:
- 前端
- JavaScript高级程序设计

top_img: https://cdn.pixabay.com/photo/2020/01/13/13/59/landscape-4762468_1280.jpg
---

这一章，我们开始从基本概念开始了解JS这门语言。

# 基本概念

一般来说，我们想要描述一门语言，都要从语法，操作符，数据类型，内置方法等方面来介绍。那么，我们就开始吧。

## 语法

毕竟JS是一个星期就写出来的编程语言，所以在语法上，它大量的借鉴了C以及其他类C语言，比如Java，Perl，的语法。

这也是经常有人问，JavaScript和Java的区别。

套用一句老师说过的话，区别就是雷锋和雷峰塔的区别。

### 1.区分大小写

这个没什么好说的，`test`和`Test`不是一个东西。

### 2.标识符

标识符，一般指 变量、函数、属性 的名字，或者函数的参数。命名规则是：

* 第一个字符必须是 `字母(a-zA-Z)`、`下划线 (_)` 、`美元符号 ($)`其中之一

* 其他字符可以是字母、下划线、美元符号或数字

### 3.注释

* 单行注释： `// 注释内容`
* 多行注释：
    `/** 多行注释 /`

### 4.严格模式
在脚本顶部第一行添加 `"use strict"`， 或者在代码体中第一行添加，即可开启。

ES5时引入的概念，作为一个编译指示，会使JavaScript引擎切换到严格模式。

### 语句
分号和花括号({、})

## 关键字和保留字
[参考 MDN 即可](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Reserved_words)

## 变量

大家都知道ECMAScript的变量是松散类型。也就是只有一个 `var` 定义所有类型。带来了方便的同时，也带来了麻烦。

之后推出了 `const` 和 `let` ,我们暂且按下不表。


## 数据类型

ES5中5中基本数据类型： `Null` , `Undefined`  ,`Boolean` , `Number` , `String` 。请各位一定要牢记！

其他的都属于类型 `object`

当然ES6之后也有其他的基本数据类型： `symbol` 我们也暂时不说。

### typeof 何许人也
大家都是 var 定义的，我们怎么区分这些数据类型呢。不要慌，ES提供了 `typeof` 操作符。

它有6个返回值：`"undefined"`，`"boolean"`，`"string"`，`"number"`，`"object"`，`"function"` 

举个栗子：

```javascript
var message = "some string";
alert(typeof message); // "string"
alert(typeof(message)); // "string"
alert(typeof 95); // "number"
```

当然也有很多让人困惑的结果，比如
```javascript
// 我虽然表示空对象，但是我自己是对象 没想到吧.jpg
typeof null // "object"  

// 我虽然表示不是数字，但是我自己是数字 没想到吧.jpg
typeof NaN // "number" 

// 不好意思 js里面没有数组数据类型，我是对象
typeof [1, 2, 3] // "object"

// class 是function的语法糖
class Foo{};
typeof Foo // "function"
```

另外要提一嘴的是
*  *`undefined`是派生于的`null`的*，所以规定 `null == undefined`

```javascript
NaN != NaN
```

一般来说，对象到字符串的转换经过了如下步骤：

  * 如果对象有 `toString()` 则调用
  * 没有的话调用 `valueOf()`
  * 如有还是无法获得原始值，则抛出类型错误异常

一般来说，对象到数字的转换过程中，类似的工作：

  * 如果对象有 `valueOf()` 
  * 没有的话调用 `toString()`
  * 如有还是无法获得原始值，则抛出类型错误异常

关于双等号引起的类型转换，请看：
[双等号引起的类型转换](https://xiangzhipeng.top/2019/08/13/%E5%8F%8C%E7%AD%89%E5%8F%B7%E5%BC%95%E8%B5%B7%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2/)


## 操作符

### 一元操作符

#### 递增和递减

递增和递减操作符直接借鉴自 C，而且各有两个版本：前置型和后置型。

`++` 和 `--`

#### 一元加和减操作符

```javascript
var num = 25;
num = +num; // 仍然是 25

var num = 25;
num = -num; // 变成了-25
```

### 位操作符

> 以下内容，平时可能很少用到。记录下来，仅供参考。

有关 [JavaScript中数值的二进制存储和二进制补码](https://xiangzhipeng.top/2019/08/08/JavaScript%E4%B8%AD%E6%95%B0%E5%80%BC%E7%9A%84%E4%BA%8C%E8%BF%9B%E5%88%B6%E5%AD%98%E5%82%A8%E5%92%8C%E4%BA%8C%E8%BF%9B%E5%88%B6%E8%A1%A5%E7%A0%81/)
可以点击连接查看。

#### 按位非 ~

返回数值的反码

```javascript
var num1 = 25; // 二进制 00000000000000000000000000011001
var num2 = ~num1; // 二进制 11111111111111111111111111100110
// 类似于
// var num2 = -num1 - 1
alert(num2); // -26
```


#### 按位与 &
从本质上讲，按位与操作就是将两个数值的每一位对齐，然后根据下表中的规则，对相同位置上的两个数执行 AND 操作：
<table>
  <tr>
    <td>第一位</td>
    <td>第二位</td>
    <td>结果</td>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>0</td>
  </tr>
  <tr>
    <td>0</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
</table>

栗子：

```javascript
var result = 25 & 3;
alert(result); //1
```

```
25 =  0000 0000 0000 0000 0000 0000 0001 1001
3 =   0000 0000 0000 0000 0000 0000 0000 0011
---------------------------------------------
AND = 0000 0000 0000 0000 0000 0000 0000 0001
```

#### 按位或 |
与按位与类似，按位或也有两个操作数，并按照一定的规则：
<table>
  <tr>
    <td>第一位</td>
    <td>第二位</td>
    <td>结果</td>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
  </tr>
  <tr>
    <td>0</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
</table>

栗子：
```javascript
var result = 25 | 3;
alert(result); //27
```
```javascript
25 = 0000 0000 0000 0000 0000 0000 0001 1001
3 =  0000 0000 0000 0000 0000 0000 0000 0011
--------------------------------------------
OR = 0000 0000 0000 0000 0000 0000 0001 1011
```

#### 按位异或 ^
同样，按位异或也有规则
<table>
  <tr>
    <td>第一位</td>
    <td>第二位</td>
    <td>结果</td>
  </tr>
  <tr>
    <td>1</td>
    <td>1</td>
    <td>0</td>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>1</td>
  </tr>
  <tr>
    <td>0</td>
    <td>1</td>
    <td>1</td>
  </tr>
  <tr>
    <td>0</td>
    <td>0</td>
    <td>0</td>
  </tr>
</table>

```javascript
var result = 25 ^ 3;
alert(result); //26
```
```javascript
25 =  0000 0000 0000 0000 0000 0000 0001 1001
3 =   0000 0000 0000 0000 0000 0000 0000 0011
---------------------------------------------
XOR = 0000 0000 0000 0000 0000 0000 0001 1010
```

#### 左移 <<
这个操作符会将数值的所有位向左移动指定的位数,不影响符号位

#### 有符号右移 >>
这个操作符会将数值向右移动，但保留符号位（即正负号标记）

#### 无符号右移 >>>
这个操作符会将数值的所有 32 位都向右移动。

> 想要具体了解的可以去Google一下


### 布尔操作符
#### 逻辑非 !
返回一个布尔值。举个栗子
```javascript
alert(!false); // true
alert(!"blue"); // false
alert(!0); // true
alert(!NaN); // true
alert(!""); // true
alert(!12345); // false
```

#### 逻辑与 &&
<table>
  <tr>
    <td>第一位</td>
    <td>第二位</td>
    <td>结果</td>
  </tr>
  <tr>
    <td>true</td>
    <td>true</td>
    <td>true</td>
  </tr>
  <tr>
    <td>true</td>
    <td>false</td>
    <td>false</td>
  </tr>
  <tr>
    <td>false</td>
    <td>true</td>
    <td>false</td>
  </tr>
  <tr>
    <td>false</td>
    <td>false</td>
    <td>false</td>
  </tr>
</table>



#### 逻辑或 ||
<table>
  <tr>
    <td>第一位</td>
    <td>第二位</td>
    <td>结果</td>
  </tr>
  <tr>
    <td>true</td>
    <td>true</td>
    <td>true</td>
  </tr>
  <tr>
    <td>true</td>
    <td>false</td>
    <td>true</td>
  </tr>
  <tr>
    <td>false</td>
    <td>true</td>
    <td>true</td>
  </tr>
  <tr>
    <td>false</td>
    <td>false</td>
    <td>false</td>
  </tr>
</table>

### 加减乘除
不做进一步说明，同数学计算，只是做一定的类型转换。

重点关注 正负的2^32的临界值

### 关系操作符
小于（ <）、大于（ >）、小于等于（ <=）和大于等于（ >=）

### 相等操作符

#### 双等号操作符

参考[双等号引起的类型转换](https://xiangzhipeng.top/2019/08/13/%E5%8F%8C%E7%AD%89%E5%8F%B7%E5%BC%95%E8%B5%B7%E7%9A%84%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2/)

#### 三等号操作符
忽略类型，比较原始的值

## 语句
### if 语句
### do-while语句
### while语句
### for语句
### for-in语句
### switch语句

## 函数
函数对任何语言来说都是一个核心的概念。通过函数可以封装任意多条语句，而且可以在任何地方、任何时候调用执行。 ECMAScript 中的函数使用 function 关键字来声明，后跟一组参数以及函数体。

没有重载
