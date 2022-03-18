---
title: 不用在每个文件中用@import引入样式变量或混入了！这里有更好的办法：@nuxtjs/style-resources
author: Xiang
tags: scss
categories: 前端
date: 2019-03-15 11:33:00
---
## Vue-Cli 下的解决方案 sass-resources-loader


如果项目使用Vue-cli 2/3，或者Vue项目用的Webpack，用这个loader都是可以的。官方对于各种场景已经写的很清楚了，请看 
[sass-resources-loader](https://github.com/shakacode/sass-resources-loader)。具体不说明了。

## Nuxt 下的解决方案  @nuxtjs/style-resources

也许你在百度的时候会搜到一些关于 nuxt-sass-resources-loader 的解决方案，但是经过自己的尝试后发现，仍然有问题无法解决，比如全局变量使用的问题。

而且在Github上，作者也说了，停止更新了。

那么推荐一个nuxt社区提供的解决方案，亲测有效，按照文档使用即可。那就是来试试 [@nuxtjs/style-resources](https://www.npmjs.com/package/@nuxtjs/style-resources) 吧。