---
title: 骨架屏css样式示例
date: 
        2020-12-08 15:47:06
tags: [骨架屏, css]
categories: 前端
---

## css代码
话不多说，先上代码

```css
@-webkit-keyframes skeleton-ani {
  0% {
    left: -100%
  }

  to {
    left: 150%
  }
}

@keyframes skeleton-ani {
  0% {
    left: -100%
  }

  to {
    left: 150%
  }
}

.pre-render-skt-loading .skeleton {
  position: relative;
  overflow: hidden;
  border: none!important;
  border-radius: 5px;
  background-color: rgba(0,0,0,0)!important;
  background-image: none!important;
  pointer-events: none;
}

.pre-render-skt-loading .skeleton:after {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  z-index: 9;
  width: 100%;
  height: 100%;
  background-color: #ebf1f8;
  display: block
}

.pre-render-skt-loading .skeleton:not(.not-round):after {
  border-radius: 4px
}

.pre-render-skt-loading .skeleton:not(.not-before):before {
  position: absolute;
  top: 0;
  width: 30%;
  height: 100%;
  content: "";
  background: -webkit-gradient(linear,left top,right top,color-stop(0,hsla(0,0%,100%,0)),color-stop(50%,hsla(0,0%,100%,.3)),to(hsla(0,0%,100%,0)));
  background: -o-linear-gradient(left,hsla(0,0%,100%,0) 0,hsla(0,0%,100%,.3) 50%,hsla(0,0%,100%,0) 100%);
  background: linear-gradient(90deg,hsla(0,0%,100%,0) 0,hsla(0,0%,100%,.3) 50%,hsla(0,0%,100%,0));
  -webkit-transform: skewX(-45deg);
  -ms-transform: skewX(-45deg);
  transform: skewX(-45deg);
  z-index: 99;
  -webkit-animation: skeleton-ani 1s ease infinite;
  animation: skeleton-ani 1s ease infinite;
  display: block
}

.pre-render-skt-loading .skeleton.badge:after {
  background-color: #f8fafc
}

```

## html 使用

```html
  <div class="pre-render-skt-loading">
    <div class="skeleton"></div>
  </div>
```
