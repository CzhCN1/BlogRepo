---
title: 更换你的Hexo主题
date: 2016-07-06 20:35:04
tags:
  - Hexo
  - NexT
categories:
  - 技术
---
## Hexo的主题
　　也许你已经厌倦了博客单调的界面，今天就来讲解如何更换博客的主题。Hexo博客框架具有非常多的优秀主题，各有各的独特风格，而且主题间更换十分方便。  
	<!-- more -->
## 更换主题
1. 首先你可以去[官方主题下载点](https://github.com/hexojs/hexo/wiki/Themes)看demo页面，挑选你心水的主题。 挑选好主题后，进入该主题的Github页面进行下载。下载的方式与之前从远端同步仓库至本地一样，可以通过clone指令或者下载zip的方法。(下面以NexT主题为例)  
![](http://7xk5u3.com1.z0.glb.clouddn.com/theme1.png)  
![](http://7xk5u3.com1.z0.glb.clouddn.com/theme2.png)  
　　将下载的主题文件夹整体移动到博客站点目录下的`themes`文件夹内。你可能发现已经有一个叫做`landscape`的文件夹，这个就是Hexo自带的模板。  
2. 打开站点目录下的`_config.yml`文件，找到theme关键字并将其后面的值修改为你想要切换的模板文件夹名称。![](http://7xk5u3.com1.z0.glb.clouddn.com/theme3.png)  
3. 依次执行`hexo clean`，`hexo g`。 
4. `hexo s` 在本地浏览你的博客，好好欣赏崭新的主题界面吧!  
## NexT主题  
　　我在挑选许久后，选择了一款叫做[NexT](http://theme-next.iissnan.com/)的主题。  
> 精于心，简于形  
 
　　这款主题很简约，也更精致。提供了很多的可配置项和第三方服务，方便你打造属于自己独一无二的博客。  
### 切换风格  
　　NexT主题提供三种风格的界面供大家选择，分别是Pisces、Mist和Muse。具体切换方法：  
1. 打开主题的配置文件`_config.yml`，这与之前的站点配置文件不同。它所在的目录是`username.github.io - themes - Next - _config.yml`.这点十分重要，一定要能区分**站点配置文件**和**主题配置文件**。  
2. 找到`Schemes`关键字，用`#`注释掉不用的主题，选择要使用的主题即可。![](http://7xk5u3.com1.z0.glb.clouddn.com/theme4.png)  
### 添加头像图片  
　　打开**站点配置文件**，添加字段`avatar: `，后面的值是你头像图片的外链。![](http://7xk5u3.com1.z0.glb.clouddn.com/theme5.png)  
　　我的外链是使用七牛云存储做图床生成的，速度快而且免费可用，推荐！
### 其他的主题配置
　　还有许多的主题配置可以修改，详细可查看官网的[帮助文档](http://theme-next.iissnan.com/theme-settings.html)。