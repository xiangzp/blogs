---
title: 让Nuxt服务用ip访问的解决方案
date: 
        2019-07-28 15:16:31
tags: 前端
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
