---
title: Js类型检测
date: 2016-07-21 20:00:04
tags:
  - JavaScript
  - 类型检测
categories:
  - 前端
  - JavaScript
---
## 前言
　　JavaScript中有很多类型检测的方法：  
　　1. typeof  
　　2. instanceof  
　　3. Object.prototype.toString  
　　4. constructor  
　　5. duck type  
  
　　各种方法都有它的用途和局限，下来我们依次分析各方法。  
	<!-- more -->
## typeof操作符
　　typeof是用于判断变量、数值字面量的类型，而且typeof是个操作符，而不是一个方法。对一个值使用typeof操作符，可能返回下列中的某个字符串：  
- "undefined"————如果这个值未定义  
- "booldean" ————如果这个值是布尔值  
- "string"	 ————如果这个值是字符串  
- "number"	 ————如果这个值是数值  
- "object"	 ————如果这个值是对象或null  
- "function" ————如果这个值是函数  
  
　　以下是在chrome浏览器控制台下的测试结果：  
![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof1.png)  
### 局限
　　我们可以看到，对于这些基本数据类型、对象和函数可以准确判断。但你可能会发现，少了一个基本数据类型null，我们也可以测试一下看看：  
![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof2.png)  
　　跟上面讲的一样，null被判断为object类型，这其实是一个历史错误。  
  
　　typeof只适用于基本类型检测。
  
## instanceof操作符
　　instanceof操作符可以检测引用类型，如果变量是给定引用类型（由构造函数表示）的实例，那么instanceof就会返回true。  
　　操作符左边的操作数必须是一个对象，而右边的操作数应该是一个函数对象或函数构造器。其原理就是判断左操作数这个对象的原型链上是否有右边这个构造函数的prototype对象属性。也就是说，instanceof是基于原型链判断类型的。  
　　以下是简单的例子：  
　　分别判断了正则表达式，数组，日期等类型，可以通过该操作符判断对象的是哪种类型的实例。  
![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof3.png)  
  
　　自己定义的构造函数和实例也可以使用该方法：  

![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof4.png)  
  
　　要注意的是，根据规定所有引用类型的值都是Object实例。因此在检测一个引用类型值和Object构造函数时，instanceof操作符始终都会返回true。如果你了解原型链，那么就很容易理解这点了。  
![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof5.png)  
  
### 局限
　　如果用instanceof操作符检测基本类型的值，则会报错。  
  
## Object.prototype.toString方法  
　　想要判断某个对象值属于哪种内置类型,最靠谱的做法就是通过Object.prototype.toString方法。该方法可以判断任何对象，并准确的给出其类型。其具体的用法为 `Object.prototype.toString.apply( )`，需要判断的内容放在括号中即可。下面是一些例子：  
 
![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof6.png)  

## constructor属性  
　　对象的constructor属性用于返回创建该对象的函数，也就是我们常说的构造函数。在JavaScript中，每个具有原型的对象都会自动获得constructor属性。除了arguments、Enumerator、Error、Global、Math、RegExp、Regular Expression等一些特殊对象之外，其他所有的JavaScript内置对象都具备constructor属性。例如：Array、Boolean、Date、Function、Number、Object、String等。  
　　因此我们可以通过判断对象的constructor属性来判断其类型，这与第二条instanceof有点类似。  
![](http://7xk5u3.com1.z0.glb.clouddn.com/typeof7.png)  

## duck type 鸭子类型 
> “当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”  
  
　　每种数据类型，都有其独有的方法或属性。那我们如果检测到被判断的对象具有这些属性或者方法的话，那我们也就可以认为这个对象的类型。