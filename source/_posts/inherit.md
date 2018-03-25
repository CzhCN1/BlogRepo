---
title: JS中的继承
date: 2016-8-7 10:00:01
tags:
  - JavaScript
  - 原型链
  - 继承
categories:
  - 前端
  - JavaScript
---
## 前言
&emsp;&emsp;之前在面试的时候被问及过这个问题，当时只回答了通过原型链继承。面试被问住是真的尴尬，现在好好总结一下JS中常用的几种继承方式。
1. 原型链
2. 借用构造函数
3. 组合继承
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

<!-- more -->
## 原型链
### 实现方式
&emsp;&emsp;原型链在上一篇博客中详细的讲解过了，它的继承方式是基于原型搜索机制进行的。依旧拿之前的例子讲解：
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

&emsp;&emsp;上例中的子类`SubType`通过原型链继承了超类`SuperType`，`instance`是子类的一个实例。子类和实例中都没有`getSuperValue()`这个方法，但是实例却可以调用该方法，这正是因为子类继承了超类的方法和属性。当调用`instance.getSuperValue()`会经历三个搜索步骤：1)搜索实例;2)搜索`SubType.prototype`;3)搜索`SuperType.prototype`，找到该方法并调用。如果仍未找到该方法，还会顺着原型链继续向上查找，直到原型链的末端才会停下来。

### 不足
&emsp;&emsp;原型链可以实现继承，而且使用方便，容易理解。但它也存在一些问题，那就是包含引用类型值得原型属性会被所有实例共享。也就是说超类的实例，在构造原型链时变成子类的原型，那超类实例中的属性也就变成了子类的原型属性。那么所有子类实例都会共享子类原型中的该属性，当一个实例修改了该属性，所有的实例都会同步该修改，这在大多数场景下是很糟糕的。

&emsp;&emsp;我们可以看下面这个简单的例子，加以理解：
```JavaScript
function SuperType(){
  this.colors = ["red","blue","green"];
}
function SubType(){

}
//SubType继承了SuperType
SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors; //["red", "blue", "green"]

var instance2 = new SubType();
instance2.colors; //["red", "blue", "green"]
instance2.colors.push("black");
instance2.colors; //["red", "blue", "green", "black"]

instance1.colors; //["red", "blue", "green", "black"]
```
&emsp;&emsp;通过例子可以看到，当`instance2`修改了它的colors属性时，也影响到了`instance1`的colors属性。

&emsp;&emsp;原型链的第二个问题是：在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。(这句话是JS高程中的一句话，不是十分理解。)

## 借用构造函数
### 实现方式
&emsp;&emsp;这种方法的基本思想就是在子类型构造函数的内部调用超类型的构造函数，因此需要使用到`apply()`或`call()`方法来借调作用域。

```JavaScript
function SuperType(name){
  this.name = name;
}
function SubType(name){
  //继承了SuperType,同时还传递了参数
  SuperType.call(this,name);
  //实例属性
  this.age = 21;
}
var instance1 = new SubType("CZH");
instance1.name; //"CZH"
instance1.age;  //"21"
instance1.hasOwnProperty("name"); //true
instance1.hasOwnProperty("age");  //true
```
&emsp;&emsp;该方法既实现了继承，也实现了传递参数的功能。而且`name`,`age`等属性都是实例的属性，不是其原型上的属性，因此修改某一实例的属性不会影响到其他的实例。

### 不足
&emsp;&emsp; 方法都在构造函数中定义，因此不利于函数复用。而且超类型的原型中定义的方法对子类型而言是不可见的，因为没有形成相应的原型链。

## 组合继承
&emsp;&emsp; 组合继承，是指将原型链和借用构造函数的技术组合到一起，结合两种方法各自优点的一种继承模式。其思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又能保证每个实例都有自己的属性。
```JavaScript
function SuperType(name){
  this.name = name;
  this.colors = ["red","blue","green"];
}
SuperType.prototype.sayName = function(){
  console.log(this.name);
}

function SubType(name,age){
  //继承属性
  SuperType.call(this,name);
  this.age = age;
}
//继承方法
SubType.prototype = new SuperType();

SubType.prototype.sayAge = function(){
  console.log(this.age);
}

var instance1 = new SubType("CZH",21);
instance1.colors;     //["red", "blue", "green"]
instance1.sayName();  // CZH
instance1.sayAge();   // 21

var instance2 = new SubType("ABC",20);
instance2.colors;     //["red", "blue", "green"]
instance2.colors.push("black");
instance2.colors;     //["red", "blue", "green", "black"]

instance1.colors;     //["red", "blue", "green"]
```

&emsp;&emsp; 子类继承了超类，子类的实例可以调用`SuperType.prototype`上的方法，也继承了`SubType`的属性。而且其属性都是实例所拥有的属性，修改一个实例的属性不会影响其他实例。很明显，该方法具有原型链和借用构造函数法两种方法的优点。

## 原型式继承
&emsp;&emsp; 这种方法的思路是借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型(后半句不能理解)。为达到这个目的，需使用以下的函数：
```JavaScript
function object(o){
  function F(){}
  F.prototype = o;
  return new F();
}
```
&emsp;&emsp;在`object()`函数内部，现创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的一个实例。从本质上讲，`object()`函数对传入其中的对象执行了一次浅复制。

```JavaScript
var person = {
  name : "CZH",
  colors : ["red","blue","green"]
};

var instance1 = object(person);
instance1.__proto__ == person;    //true
instance1.hasOwnProperty("name"); //false
```
&emsp;&emsp;你必须有一个对象可以作为另一个对象的基础，把它传递给`object()`函数，然后再根据具体需求对得到的对象加以修改即可。上例中将`person`对象作为基础对象，传入`object()`后返回的实例对象将`person()`对象作为原型。因此`person`对象的属性被所有实例共享。
## 寄生式继承
&emsp;&emsp;寄生式是与原型式继承紧密相关的一种思路。寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后返回对象。
```JavaScript
function object(o){
  function F(){}
  F.prototype = o;
  return new F();
}

function createAnother(original){
  var clone = object(original);   //创建一个新对象
  clone.sayHi = function(){       //增强对象
    console.log("hi");
  }
  return clone;                   //返回这个对象
}

var person = {
  name : "CZH",
  colors : ["red","blue","green"]
};

var instance1 = createAnother(person);
instance1.hasOwnProperty("sayHi");  //true
```
&emsp;&emsp;我的感觉这完全跟原型式继承一样，只是对新对象又进行了一次加工增强了对象而已。

## 寄生组合式继承
&emsp;&emsp;之前说过的组合继承是最常用的继承模式，但它最大的问题就是无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类原型的时候，另一次是在子类构造函数内部调用call或apply。而且子类的原型最终会包含超类对象的全部实例属性，但我们在调用子类构造函数时为每个实例重写了这些属性。也就是说有两组相同的属性，一组在子类的实例上，一组在子类的原型中，并且实例的属性屏蔽了原型中的同名属性。

&emsp;&emsp;为了解决上述问题，并具有更高的效率，就可以使用寄生组合继承的方法。其思路是：不必为了指定子类的原型而调用超类的构造函数，可以使用寄生式继承来继承超类的原型，然后再将结果指定给子类的原型。下面是使用实例：
```JavaScript
function object(o){
  function F(){}
  F.prototype = o;
  return new F();
}
//寄生式继承
function inheritPrototype(subType,superType){
  var prototype = object(superType.prototype);  //创建
  prototype.constructor = subType;              //增强
  subType.prototype = prototype;            //指定子类原型
}

function SuperType(name){
  this.name = name;
}
SuperType.prototype.sayName = function(){
  console.log(this.name);
}
function SubType(name,age){
  SuperType.call(this,name);

  this.age = age;
}
//调用寄生式继承
inheritPrototype(SubType,SuperType);

SubType.prototype.sayAge = function(){
  console.log(this.age);
}
```
&emsp;&emsp;这个例子结果与使用组合继承是完全一样的，但寄生组合式继承只调用了一次`SuperType`构造函数，并且因此避免了`SubType.prototype`上面创建不必要的、多余的属性。与此同时，原型链还能保持不变。
