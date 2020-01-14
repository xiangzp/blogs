---
title: Express 中文文档 - 快速入门 - 路由
date: 
        2019-07-31 16:39:27
tags: 前端
featured_image: https://cdn.pixabay.com/photo/2017/10/10/21/47/laptop-2838921_960_720.jpg
---
# 基本的路由
`Routing`是指确定一个应用是如何响应客户端发送到特定端点的请求，该端点是URI(或路径)和特定的HTTP请求方法(GET、POST等)。

每个路由可以有一个或多个处理程序函数，这些函数将在路由被匹配的时候执行。

路由使用一下的格式来定义：
```javascript
app.METHOD(PATH, HANDLER)
```
* `app`  代表一个`express`实例
* `METHOD`  是一个 小写的 [HTTP 请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)（译者注：如 `get`, `post`...）
* `path` 是请求路径地址
* `HANDLER ` 是在匹配路由时执行的函数

## 下面的例子说明了如何定义简单的路由。
### 在主页响应 `Hello World!`
```javascript
app.get('/', function (req, res) {
  res.send('Hello World!')
})
```

### 用 POST 方式响应根路由
```javascript
app.post('/', function (req, res) {
  res.send('Got a POST request')
})
```

### 用 PUT 方式响应 `/user`
```javascript
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})
```

### 用 PUT 方式响应 `/user`
```javascript
app.put('/user', function (req, res) {
  res.send('Got a PUT request at /user')
})
```

### 用 DELETE 方式响应 `/user`
```javascript
app.delete('/user', function (req, res) {
  res.send('Got a DELETE request at /user')
})
```

更多的参考[路由指南](https://blog.csdn.net/qq_34301371/article/details/97932280)
