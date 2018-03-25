---
title: Webpack学习笔记(一)
date: 2016-11-13 16:51:50
tags:
  - Webpack
  - 前端工具
categories:
  - 前端
---
## 前言
&emsp;&emsp;Webpack在之前学习应用React时接触过，也简单的提到过它。当时在制作苒朵音乐Web应用时，就使用了Webpack。但是当时并不理解它，只是停留在会使用的阶段，于是下载了打包好的脚手架于是就在上面开发了。如今才知道Webpack有很多便捷的功能，这也是为什么如今大小前端项目都在使用它的原因。  

## 关于Webpack
&emsp;&emsp;Webpack是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader 的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、AMD 模块、ES6 模块、CSS、图片、JSON、Coffeescript、LESS 等。  

&emsp;&emsp;简而言之，Webpack就是一个前端项目的脚手架，你的工程在它的基础上进行开发搭建会十分的方便轻松。废话不说，有什么功能我们边学边看。

## 安装
#### 安装Node环境和npm。
&emsp;&emsp;没什么好说的，大家应该都装好了。

#### 用 npm 安装 Webpack  
&emsp;&emsp;将Webpack安装到全局环境下
```
$ npm install webpack -g
```

## 使用前的准备
#### 建立一个项目的根目录文件夹，npm项目初始化
&emsp;&emsp;在根目录下用命令行执行`npm init`，并完成项目相关信息的输入。(项目名不要设为webpack、grunt、react等，否则在安装依赖时会报错)
![](http://ogir7mw53.bkt.clouddn.com/webpack/1/webpack1_1.png)
&emsp;&emsp;如果你的项目并不打算在npm上发布，那你完全可以一路回车到底。在建立结束后，你的路径下会生成一个`package.json`的配置文件。该文件记录了项目的详细信息，包括名称、版本、开发时的依赖、和运行时的依赖等等。如果感兴趣的可以百度npm详细了解，这里不再赘述。

#### 在项目中安装Webpack作为依赖包
&emsp;&emsp;在项目的根目录下运行指令
```
npm install --save-dev webpack
```
