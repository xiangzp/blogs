---
title: JavaScript中的同步异步和任务队列
date: 2020-04-21 17:16:31
tags: [JavaScript基础, 任务队列]
categories: 前端
---
![JavaScript 异步、事件循环、任务队列](https://img-blog.csdn.net/20180427162021624?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MTAxNzI0Ng==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

JS引擎是单线程的。

根据上图的流程可以看到，它负责根据事件循环按照顺序把任务放入到`栈`中执行。

执行栈遇到异步事件会把它放入到任务队列中。

任务队列又分为多个`宏队列`和一个`微队列`。

## 栈

> 栈，作为一种数据结构，是一种只能在一端进行插入和删除操作的特殊线性表。它按照`先进后出`的原则存储数据，先进入的数据被压入栈底，最后的数据在栈顶，需要读数据的时候从栈顶开始弹出数据（最后一个数据被第一个读出来）

## 任务队列

> 队列，一种特殊的线性表，特殊之处在于它只允许在表的前端（front）进行删除操作，而在表的后端（rear）进行插入操作，和栈一样，队列是一种操作受限制的线性表。又称为`先进先出`线性表。

JS中，有两类任务队列：宏任务队列（macro tasks）和微任务队列（micro tasks）。宏任务队列可以有多个，微任务队列只有一个。

### 宏任务队列

script（全局任务）, setTimeout, setInterval, setImmediate, I/O, UI rendering.

### 微任务队列

process.nextTick, Promise, Object.observer, MutationObserver.

然而微任务会先于宏任务执行。

> TODO: 待探究为何？

```javascript
setTimeout(_ => console.log(4))

  new Promise(resolve => {
    resolve()
    console.log(1)
  }).then(_ => {
    console.log(3)
  })


  setTimeout(_ => console.log(5))

  new Promise(resolve => {
    resolve()
    console.log(6)
  }).then(_ => {
    console.log(7)
  })

  console.log(2)

  /* 结果：
  * 1
  * 6
  * 2
  * 3
  * 7
  * 4
  * 5
  */
```