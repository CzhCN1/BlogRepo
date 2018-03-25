---
title: 设置Hexo不渲染source目录下的部分文件
date: 2016-07-05 16:35:04
tags:
  - Hexo
categories:
  - 技术
---
## 遇到的问题？
　　想上传几个自己写的demo到github pages，于是放到了source文件夹下。生成静态文件后，浏览时却发现这些页面都被渲染了模板样式。  

　　于是开始搜索答案，如何使某些页面跳过渲染呢？  
	<!-- more -->
## 解决方案
1. 在.md开头的文件加上以下控制符  
	`layout: none`  
然而该方法对html并没有效果。  

2. 设置`_config.yml`里的`skip_render`属性  
　　假设你的Source文件夹里面有个Demo目录，要忽略Demo目录下的所有html页面，可以通过在`_config.yml`设置`skip_render`来忽略的目录，具体如下：  
`skip_render: Demo/*.html`  

文件匹配是基于正则匹配的，更复杂的情况可以通过通配符配置该属性：  
  
- 单个文件夹下全部文件：skip_render: demo/*
- 单个文件夹下指定类型文件：skip_render: demo/*.html
- 单个文件夹下全部文件以及子目录:skip_render: demo/**
- 多个文件夹以及各种复杂情况：  
		skip_render:  
		  - demo/*.html  
		  - demo/**  

