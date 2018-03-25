---
title: 迁移你的博客至Hexo
date: 2016-07-05 16:35:04
tags:
  - Hexo
  - Git
  - Blog
categories:
  - 技术
---
## 前言
　　之前一直使用[Jekyll](http://jekyll.bootcss.com/)+GithubPages做自己的个人博客，这几天偶然机会要重做自己的博客。之前就听说了[Hexo](https://hexo.io/)这个博客系统框架，这次决定用它试一试。折腾了一晚上，踩了好多的坑，今天终于搭建成功了。  
	<!-- more -->
## HEXO简介
>A fast, simple & powerful blog framework  

　　正如官网的介绍所说，HEXO是一个快速、简单、有效的博客框架。使用之后与Jekyll相比，最大的特点就是更加方便和便捷。  

- **风一般的速度**  
Hexo基于Node.js，支持多进程，几百篇文章也可以秒生成。  

- **流畅的撰写**  
支持GitHub Flavored Markdown和所有Octopress的插件。  

- **各式的插件**  
Hexo拥有强大的插件系统，你可以通过插件支持Jade、CoffeeScript等。  
  
- **一条指令实现部署**  
你只需要一条指令，就可以将你的压面部署在GitHub Pages,Heroku或者其他站点上。  
  
## Node环境安装  
　　Jekyll基于Ruby环境，而Hexo是基于Node环境的。而在使用这些模板系统前，必须要预安装相应的环境。  
  
1. 前往[Node](http://nodejs.cn/)官网下载合适的安装包。  
2. 打开并按步骤安装Node安装包即可。  
3. 安装结束后，打开系统的CMD并输入`node -v`查看node版本号和`npm -v`查看包管理器的版本号，如果正常显示则说明安装成功。  

　　若查询版本显示没有该命令，但软件已安装完成，那可能就是node或者npm的环境变量设置有问题。现版本的环境变量无需手动添加，安装时已经自动添加。若有上述问题，可按以下步骤手动添加。  
　　1. 右键 我的电脑-属性![](http://7xk5u3.com1.z0.glb.clouddn.com/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F1.png)  


　　2. 选项卡 高级-环境变量，并按下图在用户变量中添加PATH变量。![](http://7xk5u3.com1.z0.glb.clouddn.com/NOXF7CQJ%28$%5B@%25L_9W@GYTRD.png)  

　　3. 确定后可再重复之前操作，测试指令是否正常。  
## Git环境安装  
1. 前往[Git官网](https://git-scm.com/download)，下载Windows版的Git Bash，并安装。
2. 打开Git Bash输入指令`git --version`查看git版本号，若显示正常则说明安装成功。  
3. 请提前学习git相关知识，推荐教程 [廖雪峰Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/)
  
## GitHub Pages仓库的建立  
1. 登录[GitHub官网](github.com)，并创建申请github账号
2. 创建github pages仓库，即仓库名为 username.github.io  
3. 复制该仓库的地址  
![](http://7xk5u3.com1.z0.glb.clouddn.com/%7DKCB@Q%7BM4RGA%29MKXDV%5BO%5BS2.png)  
我使用了SSH地址，也可以使用HTTPS地址。在合适的路径下运行GitBash并用`git clone`指令加仓库地址，将远程仓库克隆至本地。  

## Hexo安装初始化
1. 请确保之前的环境配置正确
2. 在之前创建的Github Pages仓库中运行GitBash
3. 依次运行指令  
`npm install hexo-cli -g`  
`hexo init`  
`npm install`  
4. 安装完毕后，可通过指令`hexo -v`查看版本，确认安装成功。  

　　此时Hexo即安装完成，可以看看当前目录下的文件分布。  

## Hexo的初次使用
1. 将你的文章（Markdown格式）放在 根目录 `source`文件夹下的`_posts`文件夹中。
2. 执行命令`hexo g`，该指令用于生成静态网页文件，运行后将产生`public`文件夹。
3. 再执行命令`hexo s`，该指令可开启本地测试服务器，通过访问`localhost:4000`，在本地测试观察你的博客。  

## 部署到Github Pages
　　Hexo自带部署指令，进行相关配置后只需要一条指令，就可以实现本地网站部署在服务器上。  
  
1. 打开根目录下的`_config.yml`文件，找到`deploy`属性按下图设置。  
![](http://7xk5u3.com1.z0.glb.clouddn.com/deploy.png)  
其中repo即仓库地址，与之前clone仓库时用的地址一致。  
2. 执行指令`npm install hexo-deployer-git --save`，安装Hexo的Git插件。
3. 依次输入以下指令，这也是之后修改博客后推送部署的步骤：  
`hexo clean` 清除缓存文件和public文件夹  
`hexo g` 生成静态文件，即public文件夹  
`hexo s` 本地测试，这步可以省略  
`hexo d` 部署指令  
4. 若部署成功，可观察github仓库是否变化。也可以直接访问你的Github Pages的网址，即你的仓库名 `username.github.io`。  
5. 若网站显示正常，那么恭喜你，创建成功！  
  　　  
如果再输入`hexo d`后报错，可能是以下几种情况：  
  - Git环境没有与github账号绑定起来，请查看GIT相关教程。
  - 没有成功安装hexo git插件，请观察根目录下的`node_modules`文件夹内是否有`hexo-deployer-git`文件夹。若没有，请执行 部署到Github Pages中的第二步。
  - 确保修改了配置文件`_config.yml`中的deploy部分。  
  - 为了稳定，请确保使用Git Bash执行上述所有的命令。作者在起初使用Node.js Command执行上述命令时就出现了部署报错的问题。  
  
## 结束语  
　　在使用中出了很多莫名的问题，最终总算是实现了博客的搭建。此次只做最基础的Hexo博客系统介绍与讲解。包括域名绑定、主题切换等其他应用，会在之后再总结出来。
>author:Czh 2016-07-05