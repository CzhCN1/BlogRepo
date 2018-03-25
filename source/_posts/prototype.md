---
title: 对原型链的简单理解
date: 2016-08-06 15:45:04
tags:
  - JavaScript
  - 原型链
categories:
  - 前端
  - JavaScript
---
## 前言
&emsp;&emsp;一直对原型和原型链有个大致的了解，日常中也偶尔会用到原型的设计模式。但总是稀里糊涂的，最近刚好在看JS高程时，决定好好深入的理解一下。

## 原型
&emsp;&emsp;只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个`prototype`属性，它的用途是包含可以由特定类型的所有实例共享的属性和方法。通过关键字`new`和构造函数调用创建的对象的原型就是构造函数的`prototype`属性的值，每个对象都是从其原型继承属性。通过`new Object()`创建的对象继承自`Object.prototype`，通过`new Array()`创建的对象继承自`Array.prototype`。
<!-- more -->

&emsp;&emsp;默认情况下，所有`prototype`属性都会自动获得一个`constructor`(构造函数)属性，这个属性包含一个指向`prototype`属性所在函数的指针。创建了自定义的构造函数后，其原型默认只会取得  `constructor`属性；至于其他方法，则都是从`Object.prototype`继承来的。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针(`__proto__`)，指向构造函数的原型属性。

&emsp;&emsp;下面举个简单的小例子：
```JavaScript
function Person(){
}

Person.prototype.name = "rason2008";
Person.prototype.age = 20;
Person.prototype.job = "student";
Person.prototype.sayName = function(){
  console.log(this.name);
}

var person1 = new Person();
person1.sayName(); //rason2008

var person2 = new Person();
person2.sayName(); //rason2008

```
![原型图例](http://7xk5u3.com1.z0.glb.clouddn.com/proto1.jpg)

&emsp;&emsp;上图展示了Person构造函数、Person的原型属性以及Person现有的两个实例之间的关系。需要注意的是，虽然两个实例都不包含属性和方法，但我们却可以调用`sayName()`方法，并且得到正确的结果，这是通过查找对象属性的过程来实现的。

## 原型链
&emsp;&emsp;通过上面的例子，可能已经理解了原型的概念，此时你可能会有疑问。`Person.prototype`有`__proto__`属性么？它的原型又是什么呢？所有函数的默认原型都是Object的实例，因此默认原型都会包含一个内部指针，指向Object.portotype。这也正是所有自定义类型都会继承`toString()`、`valueOf()`等默认方法的根本原因。

&emsp;&emsp;如果我们用`__proto__`来依次将各原型连接起来，一个原型又是另一个原型的实例，如此层层递进就构成了实例与原型的链条，也就形成了所谓的原型链。

&emsp;&emsp;原型链有什么用呢？ECMAScript只支持实现继承，而且其实现继承主要是依靠原型链来实现的。正如上面的例子，Person的实例可以调用`sayName()`方法，并且得到正确name值，这就是因为原型链的缘故。当以读取模式访问一个实例的属性时，首先会在实例中搜索该属性。如果没有找到该属性，则会继续探索实例的原型。在通过原型链实现继承的情况下，探索过程就得以沿着原型链继续向上查找。

&emsp;&emsp;下面介绍一个简单的例子：
```JavaScript
//SuperType构造函数
function SuperType(){
  this.property = true;
}
//为SuperType构造函数的原型定义方法
SuperType.prototype.getSuperValue = function(){
  return this.property;
}

//SubType构造函数
function SubType(){
  this.subproperty = false;
}
//继承了SuperType
SubType.prototype = new SuperType();
//为SubType构造函数的原型定义方法
SubType.prototype.getSubValue = function(){
  return this.subproperty;
}

var instance = new SubType();
instance.getSuperValue(); //true
instance.getSubValue(); //false

instance.constructor == SuperType; //true
instance.hasOwnProperty("constructor"); //false
SubType.prototype.hasOwnProperty("constructor");//false
```
![原型链图例](http://7xk5u3.com1.z0.glb.clouddn.com/proto2.png)

&emsp;&emsp;上例没有使用`SubType`默认提供的原型，而是给它换了一个新原型，这个新原型是`SuperType`的实例。于是，新原型不仅具有作为一个`SuperType`的实例所拥有的属性和方法，而且其内部有一个指针，指向了`SuperType`的原型。正如图所示，实例`instance`指向`SubType.prototype`,而`SubType.prototype`又指向`SuperType.prototype`。实例`instance`可以通过原型链调用方法`getSubValue()`和`getSuperValue()`。

&emsp;&emsp;值得注意的是，`instance.constructor`指向了`SuperType`,这是因为`constructor`属性并不是`instance`实例拥有的属性，而是`SuperType.prototype`的属性。`SubType.prototype.constructor`本应该指向`SubType`，可是`SubType.prototype`被`SuperType`的实例替换,因此`SubType.prototype`就没有了`constructor`属性。当访问`instance.constructor`，就会一直沿原型链查找到`SuperType.prototype`的`constructor`属性。
