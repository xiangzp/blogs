```js
var Foo = function () {}

var a = new Foo()
```

## 做了什么

1、创建了一个空的js对象（即{}）

2、将空对象的原型prototype指向构造函数的原型

3、将空对象作为构造函数的上下文（改变this指向）

4、对构造函数有返回值的判断

## 代码

```js
function myNew () {
    //1、创建一个空的对象
    let obj = {}; // let obj = Object.create({});
  

    //2、将空对象的原型prototype指向构造函数的原型
    // Object.setPrototypeOf(obj,Con.prototype);
  	obj.__proto__ = Con.prototype;
  

    //3、改变构造函数的上下文（this）,并将剩余的参数传入
    let result = Con.apply(obj,args);
  

    //4、在构造函数有返回值的情况进行判断
    return result instanceof Object ? result : obj;
}
```

