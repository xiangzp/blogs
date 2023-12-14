---
title: 一个因为前后端分离项目上线因OPTIONS请求403引发的思考
tags: 疑难记录
categories: 前端
date: 2019-02-05 19:55:00
---
最近在上线Vue+Vue Router(history模式)的前后端分离项目时，发现所有的 预检请求（preflight request）都403报错。然而后端反馈已经添加注解允许跨域（具体如何操作，本文暂不描述，可以去自行搜索）。于是排查是否是nginx不支持options请求，并成功定位问题。

于是思考如何避免这个问题：

* 1. 不使用跨域

* 2. 不发送options预检请求

最后商量决定使用第二张方法。

下面总结一下 关于options 请求的一些http知识，这些在[MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)上有具体描述。我只描述一些个人理解。

浏览器为什么要发送预检请求
规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 [GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 以外的 HTTP 请求，或者搭配某些 MIME 类型的 [POST](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 请求），浏览器必须首先使用 [OPTIONS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS) 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）。

CORS请求失败会产生错误，但是为了安全，在JavaScript代码层面是无法获知到底具体是哪里出了问题。你只能查看浏览器的控制台以得知具体是哪里出现了错误。

什么情况下发送预检请求
关于这个问题，我们要首先知道什么叫 **简单请求**

#### 使用下列方法之一：

* GET
* HEAD
* POST

#### Fetch 规范定义了[对 CORS 安全的首部字段集合](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)，不得人为设置该集合之外的其他首部字段。该集合为：

* Accept
* Accept-Language
* Content-Language
* Content-Type （需要注意额外的限制）
* DPR
* Downlink
* Save-Data
* Viewport-Width
* Width


#### Content-Type 的值仅限于下列三者之一：

* text/plain
* multipart/form-data
* application/x-www-form-urlencoded

请求中的任意XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象可以使用 XMLHttpRequest.upload 属性访问。

请求中没有使用 ReadableStream 对象。

那么对于一个简单请求，浏览器则不会发送一个预检请求来测试服务器是否允许本次跨域。

那么问题则变成把请求变成简单请求，即满足上述要求。

如果一个请求不是简单请求并且满足下述任一个条件时，浏览器都会发送 OPTIONS 预检请求。

#### 使用了下面任一 HTTP 方法：
* PUT
* DELETE
* CONNECT
* OPTIONS
* TRACE
* PATCH

#### 人为设置了对 CORS 安全的首部字段集合之外的其他首部字段。该集合为：

* Accept
* Accept-Language
* Content-Language
* Content-Type (but note the additional requirements below)
* DPR
* Downlink
* Save-Data
* Viewport-Width
* Width

 #### Content-Type 的值不属于下列之一:
 
* application/x-www-form-urlencoded
* multipart/form-data
* text/plain
* 请求中的XMLHttpRequestUpload 对象注册了任意多个事件监听器。
* 请求中使用了ReadableStream对象。


[CORS 安全的首部字段集合](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)