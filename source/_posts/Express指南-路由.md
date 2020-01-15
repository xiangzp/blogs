---
title: Express 指南-路由
date: 
        2019-07-31 16:45:15
tags: [Express, NodeJs]
categories: NodeJs
---
# 路由
路由是指应用程序的端点(uri)如何响应客户机请求。有关路由的介绍，请参阅 [基本路由](https://blog.csdn.net/qq_34301371/article/details/97929400)。

使用与HTTP方法对应的`Express app`对象的方法定义路由。例如，处理GET请求的`app.get()`和处理POST请求的`app.post`。有关完整列表，请参见app.METHOD。还可以使用app.all()处理所有HTTP方法，并使用app.use()指定中间件作为回调函数(有关详细信息，请参阅使用中间件)。
