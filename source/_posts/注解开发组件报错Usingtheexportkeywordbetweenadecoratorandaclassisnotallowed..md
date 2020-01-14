---
title: 注解开发组件 报错 Using the export keyword between a decorator and a class is not allowed.
date: 
        2019-08-30 11:05:06
tags: 前端
featured_image: https://cdn.pixabay.com/photo/2015/07/17/22/43/student-849825_960_720.jpg
---
# vue组件注解方式报错 error  Parsing error: Using the export keyword between a decorator and a class is not allowed. Please use `export @dec class` instead.  
在 `npx create-nuxt-app` 拉下来的代码中，已完成了 ts 的引入。然后想使用 `注解方式` 开发 Vue 组件的时候发现按照文档中的方式 在vscode 中被无情的标记上了红线，以及编译不通过。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190830104947261.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzAxMzcx,size_16,color_FFFFFF,t_70)
## 解决方案：
由于注解方式在当前的开发环境种只是一个 ECMA 标准的未来式，所以要在 eslint，babel，和tsconfig 中做响应的支持。
## 1.解决 eslint 报错
`eslintrc.js`
```javascript
module.exports = {
  root: true,
  env: {
    browser: true,
    node: true
  },
  parserOptions: {
    parser: 'babel-eslint',
    ecmaFeatures: {
      legacyDecorators: true
    }
  },
  ...
}
```
注意加上以下这段
```javascript
ecmaFeatures: {
  legacyDecorators: true
}
```
则告诉 eslint 我要用注解，你给我检查检查然后放行。

## 2. babel
在这次的 `create-nuxt-app` 中发现代码的依赖里少了很多东西，比如babel-core等包，然后babel的注解解析也么得了

在issue中寻找了好久，需要按照 
[https://babeljs.io/docs/en/babel-plugin-proposal-decorators](https://babeljs.io/docs/en/babel-plugin-proposal-decorators)
中所说的安装注解解析
```shell
npm install --save-dev @babel/plugin-proposal-decorators
```
其中 `decoratorsBeforeExport` 选项是默认关闭的 在 `.babelrc` 中开启即可
```
{
  "plugins": [
    ["@babel/plugin-syntax-decorators", { "decoratorsBeforeExport": true }]
  ],
  "env": {
  ...
```

## 3. typescript 支持
另外就是在 `tsconfig.json` 中指定 `experimentalDecorators` 属性，否则可能会在控制台得到
```error
Experimental support for 
decorators is a feature that is subject to change in a future 
release. Set the 'experimentalDecorators' option in your 'tsconfig' or 'jsconfig' to remove this warning.
```
在  `tsconfig.json`  中的写法是：
```json
{
  "compilerOptions": {
    ...
    "experimentalDecorators": true,
    ...
  }
}
```
