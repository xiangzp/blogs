---
title: Typora激活
tags: Typora
categories: 教程
date: 2022/12/10 22:14
---

只需要将文件

Typora.app/Contents/Resources/TypeMark/page-dist/static/js/LicenseIndex.180dd4c7.5dc16d09.chunk.js

这一行代码

e.hasActivated = "true" == e.hasActivated

改为

e.hasActivated = "true" == "true"

保存即可。