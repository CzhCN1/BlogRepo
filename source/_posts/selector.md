---
title: 原生JavaScript选择器
date: 2016-07-16 15:06:33
tags:
  - JavaScript
  - 选择器
categories:
  - 前端
  - JavaScript
---
　　jQuery是最流行的一个JavaScript库，其最重要的功能之一就是可以检索符合通过CSS选择器指定的模式的一些DOM元素，它是构建于DOM文档的CSS选择器查询之上。jQuery的出现，代替了通过`getElementById()`、`getElementsByTagName()`和`getElementsByClassName()`等选择器来检索元素。  

　　其实，原生的JavaScript中就带有非常强大的选择器`querySelector()`和`querySelectorAll()`。相比引用外部库，使用内置的API则具有更好的性能。  
	<!-- more -->  
**本次测试用HTML代码**
	```html
			<div id="myDiv">
				<div>
					<p>1</p>
				</div>
				<p>2</p>
			</div>
			<div class="selected">1</div>
			<div class="selected">2</div>
			<div class="selected">3</div>  
	```
## querySelector()方法  
　　`querySelector()`方法接受一个CSS查询并返回匹配该模式的第一个子孙元素，如果没有匹配的元素则返回null。  

　　其用法与jQuery的用法基本一致，都使用了css的选择器，它符合css选择器的规则，但是不具备过滤性功能。  
　　- `element`: 元素选择器  
　　- `#id`: ID选择器  
　　- `.class` : 类选择器  
　　- `*` : 通配选择器，选取全部子孙元素   

**JavaScript代码**：
	```javascript
		//获取body元素
		var body = document.querySelector("body");  
		//获取ID为myDiv元素  
		var myDiv = document.querySelector("#myDiv");
		//获取第一个包含类selected的元素
		var mySelect = document.querySelector(".selected");
		//获取ID为myDiv元素的子孙元素中第一个P标签
		var p = document.querySelector("#myDiv").querySelector("p");  
	```
  
　　运行结果如下：  

![](http://7xk5u3.com1.z0.glb.clouddn.com/selector1.png)  

## querySelectorAll()方法  
　　querySelectorAll()与querySelector()用法相同，其返回的值不再只是第一个元素节点，而是符合该选择器的全部元素。其返回的结果为一个节点列表，若查找结果不存在则返回一个空的列表数组。  
	```javascript
		//查找一个不存在的元素
		var my = document.querySelectorAll("#my");  
		//获取ID为myDiv元素  
		var myDiv = document.querySelectorAll("#myDiv");
		//获取包含类selected的div元素
		var mySelect = document.querySelectorAll("div.selected");
		//获取所有的P标签
		var p = document.querySelectorAll("p");  
		//获取页面中全部元素
		var all = document.querySelectorAll("*");
	```

　　运行结果如下：  

![](http://7xk5u3.com1.z0.glb.clouddn.com/selector2.png)
