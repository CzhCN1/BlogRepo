---
title: HEXO博客系统静态资源迷之404
date: 2016-11-15 16:34:05
tags:
  - Hexo
categories:
  - 技术
---
## 遇到的问题
&emsp;&emsp;好久没有看自己的博客了，在更新一篇文章后访问网站，发现内容区域全部为空白。我有点慌，赶快使用`hexo s`在本地测试，发现一切正常呀。打开控制台后发现，许多的静态资源文件请求404，多是`vender`路径下的css和js文件。登上了github，检查了仓库并没有任何问题。

## 出现问题的原因(个人臆测)
&emsp;&emsp;在搜索相关情况后，原来是最近Github Pages更新jekyll了至3.3版本，详细信息可以[点击查看](https://github.com/blog/2277-what-s-new-in-github-pages-with-jekyll-3-3)。其中有一段话如下：

>Finally, to make it easier to vendor third-party dependencies via package managers like Bundler or NPM (or Yarn), Jekyll now ignores the vendor and node_modules directories by default, speeding up build times and avoiding potential errors. If you need those directories included in your site, set exclude: [] in your site's configuration file.

&emsp;&emsp;Github Pages更新后，默认忽略`vendor`和`node_modules`目录，如果需要这些目录则需要在配置文件中设置。但明显这个更新是针对jekyll的，使用Hexo模板的明显是躺枪呀。

## 解决办法
&emsp;&emsp;最终在知乎上找到了一个解决方法，经测试后问题确实得到了解决。在这感谢原方法的作者王诚，[看原文点此处](https://www.zhihu.com/question/52268353)。  

&emsp;&emsp;在Github Pages的Help也有该问题的解决方法，[点击查看](https://help.github.com/articles/files-that-start-with-an-underscore-are-missing/)。

&emsp;&emsp;总的来说，只需要在github仓库的根目录添加一个`.nojekyll`文件，内容为`!vendors/*`。即让github pages在处理时忽略掉`vendors`文件夹，就可以解决该问题正常访问了。

&emsp;&emsp;当然，你也可以重新安装一遍hexo，最新版的hexo似乎已经修复了该问题。还有一个修改配置信息的方法，感兴趣可以[点击查看](https://huac0921.github.io/2016/11/14/aboutNext/)。

## 最后
&emsp;&emsp;突然发现，原来Jekyll才是GithubPages的亲儿子呀，hexo什么的都只是野孩子XD。
