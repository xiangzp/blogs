---
title: 用frp和阿里云服务器，在ASUS路由器上做内网穿透，并搭建一个私有云盘
tags: [frp,内网穿透,梅林固件]
categories: 瞎折腾
date: 2021-11-12 11:14:12
---

参考资料

* [如何使用 frp 实现内网穿透](https://my.oschina.net/u/4429381/blog/4280149)
* [梅林固件（ASUS AC86U）通过frp做内网穿透](https://www.cnblogs.com/jimmyfan/p/12166519.html)
* [Cloudreve文件系统](https://github.com/cloudreve/Cloudreve)
* [frp Github](https://github.com/fatedier/frp)

## frp和内网穿透

### 原理

frp 分为服务端与客户端，前者运行在有公网 IP 的服务器上，后者运行在局域网内的设备上，服务端默认会先开放 7000 端口，然后客户端与其相连。

同时客户端可以开启用于 ssh 的端口，与服务端的某个端口做映射，这样我们在终端访问服务端的端口时，会自动转发到客户端去。

![6nFFlI](https://cdn.jsdelivr.net/gh/xiangzp/picBed@master/uPic/2021/11/12/6nFFlI.jpg)

## 客户端

asus路由器，RT-AC66U

刷梅林固件，[地址](https://www.asuswrt-merlin.net/)

软件中心安装frp客户端

## 服务端

## 个人云盘