---
title: What's "this"?
date: 2016-8-9 10:32:17
tags:
  - JavaScript
  - this
  - 作用域
categories:
  - 前端
  - JavaScript
---
## this到底是什么？
&emsp;&emsp;当你深入JavaScript后会发现无处不在的this,但this到底是什么呢？当一个函数被调用时，会创建一个记录活动(执行上下文)。这个记录会包含函数在哪里被调用(调用栈)、函数的调用方式、传入的参数等信息，而this就是这个记录的一个属性。

## this到底指向什么？
&emsp;&emsp;this实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。this的绑定遵循以下的几条规则：
1. 默认绑定
2. 隐式绑定
3. 显示绑定
4. new绑定
	<!-- more -->
### 1.默认绑定
&emsp;&emsp;全局作用域中的this默认就是`window`对象，`console.log(this == window); //true`。
&emsp;&emsp;最常用的函数调用类型：全局的函数调用。
```JavaScript
function foo(){
  console.log(this.a);
}
var a = 2;
foo();  //2
```
&emsp;&emsp;函数`foo()`和变量a都是window对象下的，且函数`foo()`直接使用不带任何修饰的函数引用进行调用，从结果也可以得出：该函数调用时应用了this的默认绑定，因此this指向全局对象。需要注意的是，如果使用严格模式，则不能将全局对象用于默认绑定，因此this会绑定到`undefined`。

### 2.隐式绑定
&emsp;&emsp;一个对象内部包含一个指向函数的属性，并通过这个属性间接引用函数，从而把this间接(隐式)绑定到这个对象上。也就是只要将函数作为某个对象的方法去调用，this则指向该对象。
```JavaScript
function foo(){
  console.log(this.a);
}
var obj = {
  a : 1,
  foo : foo
}
var a = 2;

foo();      //2
obj.foo();  //1
```
&emsp;&emsp;可以看到，同一个函数使用不同的调用方式，this的绑定也出现了不同。通过`foo()`调用，采用之前讲的默认绑定规则,this指向全局对象。通过`obj.foo()`调用时，函数作为对象`obj`的方法被调用，此时this指向对象`obj`。
&emsp;&emsp;隐式绑定丢失是很常见的事，不注意的话经常会出现意想不到的结果，比如`setTimeout()`和其他回调函数，这些情况下使用this一定要小心。

### 3.显示绑定
&emsp;&emsp;利用`call()`和'apply()'等方法，强制将this绑定到需要的对象上。每个函数都包含两个非继承而来的方法:`apply()`和`call()`，这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内this对象的值。
```JavaScript
function foo(){
  console.log(this.a);
}
var obj = {
  a:2
};

foo.call(obj);  //2
```
&emsp;&emsp;在上例中，我们通过使用`call()`，在调用foo时强制把它的this绑定到obj上。这种绑定是一种显示的强制绑定，因此也称为硬绑定。

&emsp;&emsp;硬绑定十分有用，因此ES5提供了内置方法`function.prototype.bind`，用法如下：
```JavaScript
function foo(){
  console.log(this.a);
}
var obj = {
  a : 2
}
var bar = foo.bind(obj);
bar();  //2
```
&emsp;&emsp;`bind()`方法会返回一个硬编码的新函数，它会把你指定的参数设置为this的上下文并调用原始函数。

### 4.new绑定
&emsp;&emsp;首先我们需要了解使用new来调用函数，会产生一系列的操作：
1. 创建构造一个全新的对象。
2. 这个新对象会被执行`[[Prototype]](__proto__)`连接。
3. 这个新对象会绑定到函数调用的this。
4. 如果函数没有返回其他对象，那么new表达式中的函数调用会自动返回这个新对象。

&emsp;&emsp;从第三条我们就知道了，new操作会使得this绑定在这个新创建的对象上。
```
function foo(){
  this.a = 2;
}
var a = 1;
var bar = new foo();

console.log(bar.a); //2
```

## 判断this
&emsp;&emsp;四种规则，当然就会有优先级，通常判断this的方法根据优先级排序如下：
1. 判断是否在new中调用(new绑定)？如果是的话，this绑定的是新创建对象。
2. 函数是否通过call、apply(显示绑定)或硬绑定调用？如果是的话，this绑定的是指定对象。
3. 函数是否在某个上下文对象中调用(隐式绑定)？如果是的话，this绑定的是那个上下文对象。
4. 如果都不是的话，使用默认绑定，this绑定到全局对象。严格模式下，绑定到undefined。

```
function foo(){
  return function(a){
    console.log(this.a);
  }
}
var obj1 = {a:2};
var baz = foo.call(obj1);
baz();
```

## 绑定例外
&emsp;&emsp;在ES6中介绍了一种无法使用之前绑定规则的特殊函数类型：箭头函数。箭头函数并不是使用`function`关键字定义的，而是使用被称为"胖箭头"的操作符`=>`定义的。箭头函数不使用this的四种标准规则，它只根据外层作用域来决定this。看个例子：
```JavaScript
function foo(){
  return function(a){
    console.log(this.a);
  }
}
var obj1 = {
  a : 2
}
var obj2 = {
  a : 3
}
var bar = foo.call(obj1);
bar();            //undefined
bar.call(obj2);   //3
```
&emsp;&emsp;上例是使用`function`关键字定义的匿名函数作为foo函数的返回对象。在调用foo时，将this硬绑定为obj1。第一次调用bar函数`bar()`，由于是在全局作用域中调用，因此采用默认绑定，而全局作用域中没有变量a,因此返回`undefined`。第二次调用bar采用显式绑定`bar.call(obj2)`，将this绑定到`obj2`对象上,其返回结果自然为3。

&emsp;&emsp;下例则是上例的对比，foo函数返回的对象是一个箭头函数，而foo内部创建的箭头函数会捕获调用时`foo()`的this。由于`foo()`的this绑定到`obj1`,bar(引用箭头函数)的this也就会绑定到obj1,而且箭头函数的绑定无法修改(包括硬绑定和new)。
```JavaScript
function foo(){
  return (a) => {
    console.log(this.a);
  }
}
var obj1 = {
  a : 2
}
var obj2 = {
  a : 3
}
var bar = foo.call(obj1);
bar();            //2
bar.call(obj2);   //2
```
