---
title: 读JS红皮书-第四章 变量、作用域和内存问题
date: 
        2020-01-18 10:57:34
tags: [JavaScript高级程序设计,读书笔记]
categories:
- 前端
- JavaScript高级程序设计

top_img: https://cdn.pixabay.com/photo/2020/01/13/13/59/landscape-4762468_1280.jpg
---

这一章，我们来了解一些JS里的最基础的一员，那就是变量。

## 基本类型和引用类型
ECMAScript中的变量，我们按照数据类型分为：**基本类型值**和**引用类型值**

在把一个值赋值给变量的时候，解析器就会确定这个值是基本类型还是引用类型。首先我们要知道

> JavaScript是不允许直接访问内存中的位置。也就意味着都是通过变量访问，变量通过 **按值访问**和 **按引用访问** 两种方式来获取值

* 基本类型不用多说，就是之前已经介绍过的5种基本数据类型：`Undefined`、`Null`、`String`、`Number`、`Boolean`。他们是**按值访问**的。

* 引用类型是保存在内存中的变量，一般来说我们操作对象都是操作它的引用（*在为对象添加属性时，操作的是实际的对象*）他们是**按引用访问**的。

### 复制变量值

这一点表现在从一个变量向另一个变量复制基本类型值和引用类型值的时候，存在的不同。


```javascript
var num1 = 5;
var num2 = num1; // 5

var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name); //"Nicholas"
```

### 传递参数

然而在ES的函数参数中，所有的值 **都是** **按值传递** 的。没错，都是，包括引用类型。

基本类型就不用说了：
```javascript
function addTen(num) {
  num += 10;
  return num;
}
var count = 20;
var result = addTen(count);
alert(count); //20，没有变化
alert(result); //30
```
对于引用类型的值来说，我们先看个例子

```javascript
function setName(obj) {
  obj.name = "Nicholas";
  obj = new Object();
  obj.name = "Greg";
}
var person = new Object();
setName(person);
alert(person.name); //"Nicholas"
```

大家可能会认为`person.name`的值应该是`"Greg"`。这就是受到了 按引用赋值的思路的影响。

**事实上，在`setName()`函数里，参数`obj`获取到了`person`的值，也就是堆内存中的`person Object`。然后把它的 `name` 赋值为`"Nicholas"`。这个时候，`obj` 和 `person`已经没有关系了。**

**接下来又把obj的引用指向了一个局部变量 `new Object()`出的obj。所以执行结束，`obj`被销毁，`person`的`name`被赋值为`"Nicholas"`。**

**总结下来，其实方法传递的是引用类型的堆内存中的真正的值。**


### 检测类型
在介绍基本类型的时候，我们说到了`typeof`。然而在用它来检测引用类型的时候，就傻眼了。因为返回的都是`'object'`。

这个时候我们就要请出我们的新助手，`instanceof`。

它是按照原型链来识别类型的。具体关于原型链的内容稍后会做介绍。

简单来说，js引擎会在它的原型链上依次对比，是否能命中所需的类型。

```javascript
alert(person instanceof Object); // 变量 person 是 Object 吗？
alert(colors instanceof Array); // 变量 colors 是 Array 吗？
alert(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```

## 执行环境及作用域
每个函数都有自己的执行环境。全局（比如在web浏览器中就是Windows）就是最外围的执行环境。

当执行流进入一个函数时，函数的环境就会被推入一个**环境栈**中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。 ECMAScript 程序中的执行流正是由这个方便的机制控制着。

当代码在一个环境中执行时，会创建变量对象的一个**作用域链（ scope chain）**。依次来保证对所有变量和函数的有序访问。

### 延长作用域链
因为有些语句可以在作用域链的前端临时增加一个变量对象，该变量对象会在代码执行后被移除。在两种情况下会发生这种现象。具体来说，就是当执行流进入下列任何一个语句时，作用域链就会得到加长。
* try-catch 语句的 catch 块
* with 语句

### 没有块级作用域

这个说法，一般是指在使用var声明变量的时候，会自动被添加到最接近的环境中。
```javascript
if (true) {
  var color = "blue";
}
alert(color); //"blue"
```
*在ES6中，const 和 let 是可以生成块级作用域以及暂时性死区*

## 垃圾收集
详见[Javascript中的垃圾收集机制](https://xiangzhipeng.top/2019/08/10/Javascript%E4%B8%AD%E7%9A%84%E5%9E%83%E5%9C%BE%E6%94%B6%E9%9B%86%E6%9C%BA%E5%88%B6/)