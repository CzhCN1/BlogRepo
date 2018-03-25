---
title: save-dev和save的区别
date: 2016-11-13 18:02:05
tags:
  - npm
categories:
  - 前端
---
&emsp;&emsp;如果你经常用NPM安装依赖包，会注意到安装包时的指令会分`--save-dev`和`--save`两种，有什么区别呢？

&emsp;&emsp;在项目中我们通常会有一个`package.json`的配置文件，用来保存项目的相关配置信息，其中最重要的就是该项目的相关依赖包。其他用户在拿到该配置文件时，只需要用`npm install`就可以根据配置文件下载相关的依赖包，搭建好与开发者相同的环境。

&emsp;&emsp;`npm install`到底要安装什么依赖，这些都在配置文件中有标明。下图是一个`package.json`文件的内容：
![](http://ogir7mw53.bkt.clouddn.com/webpack/1/2.png)

&emsp;&emsp;我们上面提到的依赖就是图片中的`devDependencies`和`dependencies`两项。其中的`dependencies`是项目在发布或运行时所依赖的模块和包，而`devDependencies`是项目在开发时所依赖的包。  

&emsp;&emsp;就我的理解而言,`devDependencies`中的包都是开发或打包时所需的工具，而在项目打包生成要发布的文件后，`devDependencies`中的包对于生成文件就没有任何用处了。反而`dependencies`中的包是相关的类库，是发布文件正常运行的环境基础，否则项目就无法正常运行。

&emsp;&emsp;因此`webpack`类的工具应该是`devDependencies`,而`jquery`、`react`就应该是`dependencies`。

&emsp;&emsp;讲到这里你应该就明白了，`--save-dev`和`--save`的区别了。在安装包时加上`--save-dev`参数会在成功安装后，`package.json`文件的`devDependence`下会自动加上包的名称及版本。反之使用`--save`，则是在`dependencies`下添加依赖信息。

&emsp;&emsp;正常使用`npm install`时，会下载`dependencies`和`devDependencies`中的模块，当使用`npm install --production`或者注明`NODE_ENV`变量值为`production`时，只会下载`dependencies`中的模块。
