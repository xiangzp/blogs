---
title: 如何在Vue-cli设置微信分享带缩略图和描述
date: 
        2020-02-12 11:29:18
tags: [微信分享]
categories:
- 前端
- 常用代码片段
top_img: https://cdn.pixabay.com/photo/2020/01/29/12/20/cartoon-4802367_960_720.jpg
---

在做移动端h5时，难免需要在微信中分享传播。然而不处理的网址在微信中的显示效果太丑了。

那么我们就来介绍一下，如何在Vue-cli开发的项目中引入微信分享JS-SDK。

准备工作：
* 已经认证过的公众号 （交了钱的）
* 已备案的域名（项目上线的地址）
* 开发完成的项目

## 公众号相关

首先，我们登录微信公众平台，在右侧找到`公众号设置`，打开后在窗口上方选择 `功能设置`。

然后把域名填入`安全域名`，这时候，我们还要下载提示的txt文件，并按照提示把txt文件放到服务器的根目录，并确保该文件可以访问。
这样安全域名才会添加成功。

添加之后，按照提示设置 `IP白名单` 。注意，以回车分隔。

## 前端部分

### 创建 wxShare.js

```javascript
import axios from "axios";

const wx = window.wx;

export default {
  wxShowMenu: function(data) {
    const url = `${process.env.VUE_APP_API_URL}count/jssdk/jssdk.php`;

    axios
      .get(url, {
        params: {
          url: encodeURIComponent(location.href.split("#")[0])
        }
      })
      .then(function(res) {
        var getMsg = res.data;

        wx.config({
          debug: process.env.NODE_ENV === "production" ? false : true, //生产环境需要关闭debug模式

          appId: getMsg.appId, //appId通过微信服务号后台查看

          timestamp: getMsg.timestamp, //生成签名的时间戳

          nonceStr: getMsg.noncestr, //生成签名的随机字符串

          signature: getMsg.signature, //签名

          jsApiList: [
            //需要调用的JS接口列表
            "checkJsApi",
            "updateTimelineShareData",
            "updateAppMessageShareData"
          ]
        });

        wx.ready(function() {
          wx.checkJsApi({
            jsApiList: ["showMenuItems"],

            success: function() {
              wx.showMenuItems({
                menuList: [
                  "menuItem:share:appMessage", //发送给朋友
                  "menuItem:share:timeline" //分享到朋友圈
                ]
              });
            }
          });

          //分享到朋友圈

          wx.updateTimelineShareData({
            title: data.titles_circle, // 分享标题

            desc: data.descs, //分享描述

            link: data.link, // 分享链接

            imgUrl: data.imgUrl // 分享图标
          });

          //分享给朋友

          wx.updateAppMessageShareData({
            title: data.titles, // 分享标题

            desc: data.descs, //分享描述

            link: data.link, // 分享链接

            imgUrl: data.imgUrl // 分享图标
          });
        });
      });
  }
};

```

### 在`main.js`调用

```javascript
import WxShare from "@/utils/wxshare";

Vue.prototype.$WxShare = WxShare;
```

### 在组件中使用

```javascript
export default {
  mounted() {
    //设置微信分享信息
    this.$WxShare.wxShowMenu({
        titles: "分享的消息标题",
        titles_circle: "分享到朋友圈标题",
        descs: "描述",
        link: "导航链接",
        imgUrl: "可直接访问的缩略图url地址"
    });
  }
} 
```

### 备注

在 `wxshare.js`里不是只可以设置两个api，更多的请参考[微信开放文档|JS-SDK说明文档](https://developers.weixin.qq.com/doc/offiaccount/OA_Web_Apps/JS-SDK.html)