---
title: 读红皮书，总结一下JavaScript中的继承
date: 
        2019-04-23 11:05:06
tags: [JavaScript基础, 继承]
categories: 前端
---

## 对象和继承
红皮书在第6章重点介绍了`面向对象的程序设计`。在JavaScript中没有类的概念，对于ECMAScript来说，对象基于一个引用类型创建。

在第一小节，详细介绍了对象的创建方式，其实对后面所说的继承有很大的帮助。比如Object.defineProperty，工厂模式，构造模式，原型模式等等，都是继承的基础。略去不表。

我们着重来看一下继承。

第二节循序渐进的介绍了继承，以及从各种创建对象的方式上来对继承方式进行划分。

## 继承的种类
- 原型链继承
- 构造继承
- 组合继承
- 原型继承
- 寄生继承
- 寄生组合继承

## 原型链继承

缺点：
- 只能统一在给Child的原型赋值时调用父类的构造方法。也就意味着，Child的实例在创建时无法传递参数。new Child(args) args 不能作用在父类上
- Parent的构造属性会因为new关键词变成Child的原型属性。那么会导致所有的实例共享该属性。如果该属性是引用类型，比如数组，那么一个实例修改了属性，所有的实例的该属性都会变化


重点代码:
```js
  Child.prototype = new Parent(30)
```

实现代码：

```js
// 原型链继承
  function Parent (age) {
    this.name = '父亲'
    this.relations = ['1', '2', '3']
    this.age = age
  }
  Parent.prototype.age = 30
  Parent.prototype.getName = function () {
    console.log(this.name)
  }

  function Child () {
    this.name = '儿子'
  }
  Child.prototype = new Parent(30)

  // 缺点：只能统一在给Child的原型赋值时调用父类的构造方法
  // 也就意味着，Child的实例在创建时无法传递参数
  // new Child(args) args 不能作用在父类上
  let child = new Child()
  // 缺点：Parent的构造属性会因为new关键词变成Child的原型属性
  // 那么会导致所有的实例共享该属性
  // 如果该属性是引用类型，比如数组，那么一个实例修改了属性 所有的实例的该属性都会变化
  child.relations.push('4')
  
  child.getName() // '儿子'

  console.log(new Child().relations) // ["1", "2", "3", "4"]
  console.log(new Child().relations === new Child().__proto__.relations) // true
```

## 借用构造函数继承（伪造对象、经典继承）

缺点：方法不能得到重用，每创建一个实例则创建一个方法

优势：
- 可以像构造函数传递参数，生成不同的子类实例
- 在子类构造函数执行时，用子类的对象执行父类的构造方法，在子类中创建了属性的副本。该副本不受引用类型影响，解决了原型方法这方面的问题

重点代码:
```js
  function Child (name, age) {
    Parent.apply(this, arguments);
  }
```

实现代码：

```js
  // 借用构造函数继承（伪造对象、经典继承）
  function Parent (name, age) {
    this.name = name
    this.age = age
    this.colors = ['1', '2', '3']
    // 缺点：方法不能得到重用，每创建一个实例则创建一个方法
    this.getName = function() {
      console.log(this.name)
    }
  }

  function Child (name, age) {
    Parent.apply(this, arguments);
  }

  let child = new Child('x', 10)
  child.colors.push('4')

  console.log(child.name) // x
  console.log(child.colors) // ["1", "2", "3", "4"]
  child.getName() // x

  // 优势：1. 可以像构造函数传递参数，生成不同的子类实例
  // 2. 在子类构造函数执行时，用子类的对象执行父类的构造方法，在子类中创建了属性的副本
  // 该副本不受引用类型影响，解决了原型方法这方面的问题
  console.log(new Child('y').colors) // ["1", "2", "3"]

```

## 组合继承

结合了原型链继承和构造继承的优点，是最受欢迎的继承方法。

优点:
- 利用构造继承的原理，解决了引用类型的问题
- 利用了原型方法作为引用类型可以复用的问题,也修改了Child原型的构造函数指向问题

缺点：两次执行了父类构造函数

重点代码:
```js
  Parent.apply(this, arguments)
  
  ...
  
  Child.prototype = new Parent()
  Child.prototype.constructor = Child
```

实现代码：

```js
  // 组合继承
  function Parent (name, age) {
    this.name = name
    this.age = age
    this.colors = ['1', '2', '3']
  }
  Parent.prototype.getName = function () {
    return this.name
  }

  function Child (name, age) {
    // 第 2 次调用Parent
    // 缺点: 多次调用了父类的构造函数
    Parent.apply(this, arguments)
  }
  
  // 第 1 次调用Parent
  // 缺点: 多次调用了父类的构造函数
  Child.prototype = new Parent()
  // 优势：完成了原型方法作为引用类型可以复用的问题
  // 也修改了Child原型的构造函数指向问题
  Child.prototype.constructor = Child

  let child = new Child('x', 1)
  child.colors.push('4')

  console.log(child.name) // x
  console.log(child.getName()) // x
  console.log(child.colors) // ["1", "2", "3", "4"]
  
  let child2 = new Child('y', 22)

  // 优势： 利用构造继承的原理，解决了引用类型的问题
  console.log(child2.colors) // ["1", "2", "3"]
```

## 原型式继承

实现代码：

```js
  // 原型式继承
  function object (o) {
    function F () {}
    F.prototype = o
    return new F
  }

  var parent = {
    a: 1,
    b: [1, 2, 3]
  }

  // 优势：只用来让对象保持一致的情况下使用，简单方便
  // 缺点：和原型链继承一样，无法修改引用类型
  // 无法用类型检查等类操作
  var child = object(parent)

  console.log(child.a) // 1
  child.b.push(4)
  console.log(parent.b) // [1, 2, 3, 4]
```

## 寄生继承

所谓的 `寄生`，在我理解看来则是，看起来创建了一个新的对象，实际上它的属性都依赖于传入的对象，并不归自己所有。在此基础上，设置了自己的属性，并把属于自己的和不属于的一股脑都告诉别人，这便是一种寄生，寄生于传入的对象。

```js
  // 寄生继承
  function object (o) {
    function F () {}
    F.prototype = o
    return new F
  }

  function createAnother(o) {
    let clone = object(o)
    clone.c = 3
    return clone
  }

  var parent = {
    a: 1,
    b: [1, 2, 3]
  }

  // 优势：只用来让对象保持一致的情况下使用，简单方便
  // 缺点：和原型链继承一样，无法修改引用类型
  // 无法用类型检查等类操作
  var child = createAnother(parent)

  console.log(child.a) // 1
  console.log(child.c) // 3
  child.b.push(4)
  console.log(parent.b) // [1, 2, 3, 4]
```

## 寄生组合继承
结合了原型和寄生的优点，利用了组合继承，把第一次用new执行父类构造函数体替换为对对象的原型复制的方法

**只复制了父类原型上的引用类型，而抛弃了的构造属性则通过构造继承的方式获取。**

这样便是 **最理想的继承范式**


重点代码:
```js
  Parent.apply(this, arguments)
  
  ...
  
  inheritPrototype(Parent, Child)
```

实现代码：

```js
  // 寄生组合继承
  function Parent (name, age) {
    this.name = name
    this.age = age
    this.colors = ['1', '2', '3']
  }
  Parent.prototype.getName = function () {
    return this.name
  }

  function Child (name, age) {
    // 第 2 次调用Parent
    // 缺点: 多次调用了父类的构造函数
    Parent.apply(this, arguments)
  }

  function object (o) {
    function F () {}
    F.prototype = o
    // new 关键词操作后 范围一个对象
    // constructor 丢失
    return new F() // { __proto__: Parent.prototype }
  }

  function inheritPrototype (Parent, Child) {
    // 父类的原型对象中，只包含了方法等引用对象，而不包含构造函数内的属性 name, age, colors这些属性
    // 故 节省了子类继承后 由于构造继承生成的构造属性
    let _prototype = object(Parent.prototype) 
    _prototype.constructor = Child
    Child.prototype = _prototype // { __proto__: Parent.prototype，constructor: Child }
  }
  
  // 利用了寄生继承的原理
  // 生成了一个原型对象，他的原型指向父类的原型对象，构造函数指向子类构造函数
  // 并将该构造函数赋值给子类的原型对象
  inheritPrototype(Parent, Child)

  let child = new Child('x', 1)
  child.colors.push('4')

  console.log(child.name) // x
  console.log(child.getName()) // x
  console.log(child.colors) // ["1", "2", "3", "4"]
  
  let child2 = new Child('y', 22)

  // 优势： 利用构造继承的原理，解决了引用类型的问题
  console.log(child2.colors) // ["1", "2", "3"]
```