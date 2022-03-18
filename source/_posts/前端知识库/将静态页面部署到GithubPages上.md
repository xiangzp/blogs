---
title: 将静态页面部署到GithubPages上
date: 
        2020-01-14 11:29:18
tags: [Github,域名,部署]
categories:
- 前端
- 瞎折腾
top_img: https://cdn.pixabay.com/photo/2017/01/03/18/51/lost-places-1950246_960_720.jpg
---
## 准备
开发好的静态网站

##  创建仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114111504747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzAxMzcx,size_16,color_FFFFFF,t_70)
注意， 否则访问：
1.  仓库名和用户名保持相同
2.  除非是github Pro用户，否则设置仓库为Public

##  克隆仓库，上传代码

##  设置自定义域名

 先去域名供应商（比如阿里云），购买一个域名。注意 	`.com, .cn`等域名需要实名认证，请自行注意，否则无法使用。

 准备好域名后，先 `ping` 你的仓库的 域名 `https://username.github.io`。这步是为了获取到你的仓库的ip。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114112341613.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzAxMzcx,size_16,color_FFFFFF,t_70)
然后在域名服务商的 域名解析 ->  解析设置 中，把你获得的ip填写到域名解析规则中，也就是添加记录。将你的IP填入后，会生成两条主机记录。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200114112733260.png)

稍等就可以通过自定义域名访问了。
