---
title: 渐进增强和优雅降级
date: 2016-9-5 22:32:14
tags:
  - 浏览器
categories:
  - 前端
---
## 前言
&emsp;&emsp;刚做完某东的在线笔试，答得一塌糊涂。有个问题是渐进增强和优雅降级的概念，甚至之前都似乎没听过，于是赶紧查了资料补一下。

## 概念
**渐进增强 (progressive enhancement)**：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。
```css
.transition{
    transition: all .5s;          /* 标准写法 */
    -moz-transition: all .5s;     /* firefox 内核 */
    -webkit-transition: all .5s;  /* webkit 内核 */
}
```

**优雅降级 (graceful degradation)**：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。
```css
.transition{
    -webkit-transition: all .5s;  /* webkit 内核 */
    -moz-transition: all .5s;     /* firefox 内核 */
    transition: all .5s;          /* 标准写法 */
}
```

## 区别
&emsp;&emsp;优雅降级是从复杂的现状开始，并试图减少用户体验的供给，而渐进增强则是从一个非常基础的，能够起作用的版本开始，并不断扩充，以适应未来环境的需要。
&emsp;&emsp;除此之外，我认为两者没有太大的区别。随着CSS3等技术在各个浏览器都得到更好的兼容和支持后，这两种方式也会被逐级淡化。然而渐进增强和优雅降级是两种优秀的思想，无论哪种都使得页面尽量兼容更多的浏览器，得到更好的表现效果。
