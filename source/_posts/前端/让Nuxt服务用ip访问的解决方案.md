---
title: 让Nuxt服务用ip访问的解决方案
date: 
        2019-07-28 15:16:31
tags: [Vue,Nuxt]
categories: 前端
top_img: https://cdn.pixabay.com/photo/2017/08/28/21/12/yellow-ipe-2691268_960_720.jpg
---
# 如何解决？

如果你想用ip来访问nuxt建立的本地服务，可以在 `package.json` 添加以下代码：

```json
...
"config": {
  "nuxt": {
    "host": "0.0.0.0",
    "port": 3000
  }
},
...
```
