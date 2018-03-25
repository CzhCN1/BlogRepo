---
title: 事件绑定的几种方法
date: 2016-07-10 16:35:04
tags:
  - JavaScript
  - 事件代理
  - 闭包
categories:
  - 前端
  - JavaScript
---
　　在为大量元素重复绑定相同事件时，会有几种不同的方法，不同的方法各有优缺点。在这里我会分别对几种方法进行介绍和讲解！  
　　现在，我们要为下面的无序列表中的每个列表项`li`都绑定一个点击事件，打印出其内容。
	```html
		<ul id="test">
			<li>第一行</li>
			<li>第二行</li>
			<li>第三行</li>
		</ul>  
	```
　　页面的显示如下：
![](http://7xk5u3.com1.z0.glb.clouddn.com/agent2.png)  
	<!-- more -->
## 循环绑定事件  
　　这种方式是最容易想到的方法，即遍历所有需要绑定事件的元素，将事件绑定在对应元素上。其实这里有个大坑！  
	```javascript
		var ul = document.getElementById('test'),
			li = ul.getElementsByTagName('li');
		for(var i = 0;i < li.length;i++){
			li[i].onclick = function(){
				console.log(li[i].innerHTML);
			};
		}  
  	```
　　当我们点击后，会产生以下错误：
![](http://7xk5u3.com1.z0.glb.clouddn.com/agent1.png)  
　　这样你可能还有点蒙，我们对程序做以下修改：  
　　将循环判断条件由 `i<li.length`改为`i<li.length-1`,此时我们相当于少绑定了一个元素的事件。我们保存程序后再点击测试一下。  
![](http://7xk5u3.com1.z0.glb.clouddn.com/agent3.png)  
　　发现当点击第一行和第二行的li时，都会打印出第三行的信息。而在点击第三行时，没有任何反应，这点倒是与我们之前的推断一致。可为什么点击第一行、第二行却显示第三行的内容呢？  
　　这就是我刚所说的坑！在绑定事件时，循环变量i一直自加，直到最后加一等于了数组的长度，从而终止了循环。因此在执行完事件绑定后，i的值应为数组的长度，而数组是从下标0开始，以数组长度值-1结束。  
　　当事件绑定结束后，i一直保持为结束时的值不变。当事件触发后，li[i]就不再是触发事件的元素了，而是`li[li.length]`这个元素。因此就会出现我们第一次的那个错误，因为`li[li.length]`是undefined。之后将循环次数减一，不管哪个元素触发事件都会显示`li[li.length-1]`元素的内容，因此不管点击第一行还是第二行都会显示第三行的内容。  
### 解决方法  
1. 使用this  
　　将`console.log(li[i].innerHTML);`更改为`console.log(this.innerHTML);`。即用`this`取代`li[i]`,this是JavaScript中非常重要的关键字，之后会在博客中详细介绍。这里的this代表的是触发事件的对象，我们的事件绑定在`li`元素上。当事件触发，this会指向这个事件触发的对象，也就是我们需要的li元素了。
2. 使用闭包  
	```javascript
		var ul = document.getElementById('test'),
			li = ul.getElementsByTagName('li');
		for(var i = 0;i < li.length;i++){
			(function(){
				var j = i;
				li[j].onclick = function(){
					console.log(li[j].innerHTML);
				};
			})();
		}
	```
　　在for循环内部，使用一个立即执行函数。一个临时变量j保存i的值，在该函数内部再绑定事件。事件的绑定函数与外层的立即执行函数形成闭包，内部函数可以读取外部函数的变量值，且外部函数会为内部函数保留变量值不清楚。因此，在事件触发后，触发事件的服务函数会通过闭包读取变老了j，而j保留着当时的i值。  
## 使用事件代理  
　　在面对我们这个例子的情况时，通常是使用事件代理而非采用循环绑定。那事件代理相比前者的优点在哪呢？  
　　如果我们的列表项非常多，那么每个li都绑定一个事件，那就非常占用系统的资源和时间。而且如果动态的向ul列表中添加了li的DOM节点，用之前的方法是无法触发事件的。因为新添加的li节点是在绑定事件之后，即新添加的li节点是没有绑定上事件的。事件代理则解决了以上两个问题，具体代码如下：  
	```javascript
		var ul = document.getElementById('test');
		ul.onclick = function(e){
			var eve = e || window.e;
			if(e.target.tagName == "LI"){
				console.log(e.target.innerHTML);
			}
		};  
  	```
　　事件代理，即把事件绑定在其公共的父元素上。由于事件流的作用，当子节点接收到触发事件，其父节点也会同样接收到该事件。因此我们只需在其父节点上绑定事件，即可侦听其所有子节点的事件，然后只需通过判断事件触发的对象是否为li元素即可。