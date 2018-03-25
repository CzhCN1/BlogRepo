---
title: React项目开发(1)
date: 2016-8-12 15:32:00
tags:
  - React
  - yeoman
  - Webpack
categories:
  - 前端
  - React
---
## 一个新的React项目
&emsp;&emsp;最近刚开始学习React,所以打算基于React制作一个项目来推动自己学习。SegmentFault社区看到很多人问如何学习React?有一个答案令我印象深刻，“学自行车最好的方法就是骑上去”。

&emsp;&emsp;想了想，打算做一个类似音乐播放器的单页Web应用。每天只随机推荐几首音乐，不提供搜索功能，尽量做出漂亮些的界面和动画效果。

&emsp;&emsp;打算使用React ES6的写法，最近一直看的都是ES5的方法，但技术总是在进步的，ES6带来的便利是值得我们去拥抱的。至于Flux、Reflux、Redux还未接触，边学边看吧。
<!-- more -->
## 搭建脚手架
&emsp;&emsp;采用Webpack前端模块化工具管理我们的项目，有各种各样的loader让我们可以用require加载资源。Webpack和React结合的另一个强大的地方就是，在修改了组件源码之后，不刷新页面就能把修改同步到页面上，也就是传说的"hot-loader"技术。

&emsp;&emsp;刚接触Webpack工具，对各种配置都不熟悉，所以我偷懒使用了yeoman。yeoman是一个前端脚手架生成的工具，可以使用生成器'generator'生成所需要的脚手架，这对还不是很了解Webpack的我来说简直是福音。

### 安装
&emsp;&emsp;安装十分简单，只需要几条指令就好。根据项目需要，我们选择的是`react-webpack`生成器。更多的生成器资料可以查看 **[官方网站](http://yeoman.io/generators/)**。运行一下指令完成安装:
```
# Make sure both is installed globally
npm install -g yo
npm install -g generator-react-webpack
```

### 运行
&emsp;&emsp;进入到项目文件夹下，运行以下指令开始生成脚手架：
```
yo react-webpack
```
&emsp;&emsp;短暂等待后，它会让你做一些选择配置项目，如项目名字、样式文件格式等。这里需要用方向键做一些选择，但是windows下的Git Bash不能操控，我使用的是Node.js Command，完成这里的操作，不知是我的问题还是bug。随后便是漫长的等待。。。

&emsp;&emsp;这里还有个问题，如果选择使用sass、scss需要安装`node-sass`模块。似乎是由于国内的网络问题，安装会失败，这导致项目不能识别scss、sass文件。因此这里需要在生成结束后，手动安装该模块。这里我们使用国内的镜像，输入下面的指令：
```
cnpm install node-sass
```
&emsp;&emsp;当然了，你首先要安装CNPM啦，如果你安装过就当我没说吧。。
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 生成内容
&emsp;&emsp;最终项目的目录结构如下图：

![生成内容](http://7xk5u3.com1.z0.glb.clouddn.com/react1-1.png)

&emsp;&emsp;安装完我一看，好多地方和之前的都不一样了！我一看package.json，原来是更新到Webpack2.0了。。

### 本地测试
&emsp;&emsp;到底安装好没，我们需要在本地测试一下。输入下面代码：
```
npm start
```
&emsp;&emsp;运行后会自动打开页面`http://localhost:8000/`，如果没有可以手动打开。如果看到了YEOMAN的标志则说明环境搭建正常。可以找到`src/components/App.js`并进行编辑，保存后就能看到页面上随之变化了。神奇吧！这就是我们刚才说的`hot-loader`技术。
&emsp;&emsp;我们之后主要就是在App.js下面进行编程，不废话了，开始吧。
