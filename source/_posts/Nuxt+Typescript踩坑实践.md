---
title: Nuxt +Typescript 踩坑实践
date: 
        2019-08-30 11:17:34
tags: [Vue,Nuxt,TypeScript]
categories: 前端
featured_image: https://cdn.pixabay.com/photo/2016/02/19/10/15/typewriter-1209140_960_720.jpg
---
>最近想用 nuxt + typescript 搭建一个基于 Vue服务端渲染 的前端框架。然而随着最近的更新踩了一路的坑，记录一下。

# Typescript 支持
在 nuxt 的中文文档中，明确说明了如何使用 Typescript，然而跟着文档开始之后一路报错，比如 ts无法编译等等等 垃圾！然后才发现，在英文文档中别有洞天。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830110826638.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzAxMzcx,size_16,color_FFFFFF,t_70)
英文文档
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830110958888.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzAxMzcx,size_16,color_FFFFFF,t_70)
即 Nuxt 提供了 TS 的 `TypeScript Support for Nuxt.js`
[地址是这个：https://typescript.nuxtjs.org/](https://typescript.nuxtjs.org/)
发现除了英文文档，其他翻译的文档均没有更新！！！真的是，学会英语的重要性啊！！！大家一定要优先看英文文档。

按照 `Guide` 中的指南，**在 `create-nuxt-app` 创建的代码框架的基础上** 安装依赖等，就可以完成  **Typescript 支持**， **配置项更改** ， **Module options** ，**运行时配置**，**lint配置** 等。


# 注解开发组件报错
可以参考

[注解开发组件 报错 Using the export keyword between a decorator and a class is not allowed.](https://blog.csdn.net/qq_34301371/article/details/100152565)

# Sass 文件还需要每个文件导入？太累了
以往的scss文件中指定的 变量文件var 和 混入mixins 文件，可能在用到的时候需要在每个文件导入，觉得很麻烦。
nuxt 中提供了一种别的解决方案，可以一次性导入一些样式的资源resources，以后在组件中需要用到的时候，写变量名，混入就可以直接使用了，是不是贼爽！
## 如何使用呢？看这里 [https://blog.csdn.net/qq_34301371/article/details/97614473](https://blog.csdn.net/qq_34301371/article/details/97614473)
