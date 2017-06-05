---
title: javascript中的this详解
date: 2016-10-19 08:34:11
tags: javascript
categories: javascript
---
## 概念

this关键字是javascript中很重要的概念，正确掌握了 JavaScript 中的 this 关键字，才算迈入了 JavaScript 这门语言的门槛。在 Java 等面向对象的语言中，this 关键字的含义是明确且具体的，即指代当前对象。一般在编译期确定下来，或称为编译期绑定。而在 JavaScript中，this是动态绑定，或称为运行期绑定的，这就导致JavaScript中的this关键字有能力具备多重含义，带来灵活性的同时，也为初学者带来不少困惑。

## javascript中this

前面说过，javascript中this在函数定义的时候是无法确定指向的，只有在函数执行的时候才能确定。它可以是全局对象、当前对象或者任意对象，这完全取决于函数的调用方式。
JavaScript 中函数的调用有以下几种方式：作为对象方法调用，作为函数调用，作为构造函数调用，和使用 apply 或 call 调用。

1.作为对象方法调用

在 JavaScript 中，函数也是对象，因此函数可以作为一个对象的属性，此时该函数被称为该对象的方法，在使用这种调用方式时，this 被自然绑定到该对象。
``` bash
var point = {
 x : 0,
 y : 0,
 moveTo : function(x, y) {
     this.x = this.x + x;
     this.y = this.y + y;
     }
 };

 point.moveTo(1, 1)//this 绑定到当前对象，即 point 对象
 ```

2.作为函数调用

 函数也可以直接被调用，此时 this 绑定到全局对象。在浏览器中，非严格模式下window 就是该全局对象，而严格模式下是undefine。
 ``` bash
 function makeNoSense(x) {
this.x = x;
}

makeNoSense(5);
x;// x 已经成为一个值为 5 的全局变量
```
对于内部函数，即声明在另外一个函数体内的函数，这种绑定到全局对象的方式会产生另外一个问题。我们仍然以前面提到的 point 对象为例，这次我们希望在 moveTo 方法内定义两个函数，分别将 x，y 坐标进行平移。结果可能出乎大家意料，不仅 point 对象没有移动，反而多出两个全局变量 x，y。
 ``` bash
var point = {
 x : 0,
 y : 0,
 moveTo : function(x, y) {
     // 内部函数
     var moveX = function(x) {
     this.x = x;//this 绑定到了哪里？
    };
    // 内部函数
    var moveY = function(y) {
    this.y = y;//this 绑定到了哪里？
    };

    moveX(x);
    moveY(y);
    }
 };
 point.moveTo(1, 1);
 point.x; //==>0
 point.y; //==>0
 x; //==>1
 y; //==>1
 ```
 这属于 JavaScript 的设计缺陷，正确的设计方式是内部函数的 this 应该绑定到其外层函数对应的对象上，为了规避这一设计缺陷，聪明的 JavaScript 程序员想出了变量替代的方法，约定俗成，该变量一般被命名为 that。
 ``` bash
 var point = {
 x : 0,
 y : 0,
 moveTo : function(x, y) {
      var that = this;
     // 内部函数
     var moveX = function(x) {
     that.x = x;
     };
     // 内部函数
     var moveY = function(y) {
     that.y = y;
     }
     moveX(x);
     moveY(y);
     }
 };
 point.moveTo(1, 1);
 point.x; //==>1
 point.y; //==>1
```
3.作为构造函数调用

JavaScript 支持面向对象式编程，与主流的面向对象式编程语言不同，JavaScript 并没有类（class）的概念，而是使用基于原型（prototype）的继承方式。相应的，JavaScript 中的构造函数也很特殊，如果不使用 new 调用，则和普通函数一样。作为又一项约定俗成的准则，构造函数以大写字母开头，提醒调用者使用正确的方式调用。如果调用正确，this 绑定到新创建的对象上。
 ``` bash
function Point(x, y){
   this.x = x;
   this.y = y;
}
var p = new Point(20,50);
console.log(a.x);//20
```
上面的示例，p 是Point通过new关键字创建的一个实例，这个时候this指向的是p ，所以p可以点出函数Point里面的x。我们这里用变量p创建了一个Point的实例（相当于复制了一份Point到对象p里面），此时仅仅只是创建，并没有执行，而调用这个函数Point的是对象p，那么this指向的自然是对象p，那么为什么对象p中会有x，因为你已经复制了一份Point函数到对象p中，用了new关键字就等同于复制了一份。

具体原理：
首先new关键字会创建一个空的对象，然后会自动调用一个函数apply方法，将this指向这个空对象，这样的话函数内部的this就会被这个空的对象替代。
* 当this遇到return时
return一个对象{}
 ``` bash  
function fn()  
{  
    this.user = 'jimmy';  
    return {};  
}
var a = new fn;  
console.log(a.user); //undefined
```
return一个function
 ``` bash  
function fn()  
{  
    this.user = 'jimmy';  
    return function(){};
}
var a = new fn;  
console.log(a.user); //undefined
```
return一个基本类型变量
 ``` bash
function fn()  
{  
    this.user = 'jimmy';  
    return 1;
}
var a = new fn;  
console.log(a.user); //jimmy
```
return一个undefined
 ``` bash
function fn()  
{  
    this.user = 'jimmy';  
    return undefined;
}
var a = new fn;  
console.log(a.user); //jimmy
```
return一个null
 ``` bash
function fn()  
{  
    this.user = '追梦子';  
    return null;
}
var a = new fn;  
console.log(a.user); //追梦子
```
以上总结如下：
如果返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数的实例。虽然null也是对象，但是在这里this还是指向那个函数的实例，因为null比较特殊。

4.使用 apply 或 call 调用

让我们再一次重申，在 JavaScript 中函数也是对象，对象则有方法，apply 和 call 就是函数对象的方法。这两个方法异常强大，他们允许切换函数执行的上下文环境（context），即 this 绑定的对象。很多 JavaScript 中的技巧以及类库都用到了该方法。让我们看一个具体的例子：
 ``` bash
function Point(x, y){
    this.x = x;
    this.y = y;
    this.moveTo = function(x, y){
        this.x = x;
        this.y = y;
    }
 }

 var p1 = new Point(0, 0);
 var p2 = {x: 0, y: 0};
 p1.moveTo(1, 1);
 p1.moveTo.apply(p2, [10, 10]);
 ```
 在上面的例子中，我们使用构造函数生成了一个对象 p1，该对象同时具有 moveTo 方法；使用对象字面量创建了另一个对象 p2，我们看到使用 apply 可以将 p1 的方法应用到 p2 上，这时候 this 也被绑定到对象 p2 上。另一个方法 call 也具备同样功能，不同的是最后的参数不是作为一个数组统一传入，而是分开传入的。

5.箭头函数中的this

在 ES6 的新规范中，加入了箭头函数，它和普通函数最不一样的一点就是 this 的指向了，上文我们使用闭包来解决 this 的指向，如果用上了箭头函数就可以更完美的解决了：
 ``` bash
var obj = {
  name: 'qiutc',
  foo: function() {
    console.log(this);
  },
  foo2: function() {
    console.log(this);
    setTimeout(() => {
      console.log(this);  // Object {name: "qiutc"}
    }, 1000);
  }
}

obj.foo2();
```
可以看到，在 setTimeout 执行的函数中，本应该打印出在 Window，但是在这里 this 却指向了 obj，原因就在于，给 setTimeout 传入的函数（参数）是一个箭头函数：函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
简单来说， 箭头函数中的 this 只和定义它时候的作用域的 this 有关，而与在哪里以及如何调用它无关，同时它的 this 指向是不可改变的。

6.setTimeout中的this

 * 严格模式下的坑

 如果直接执行回调函数而没有绑定作用域，那么它的 this 是指向全局对象(window)，在严格模式下会指向 undefined，然而在setTimeout 中的回调函数在严格模式下却表现出不同：
``` bash
  'use strict';
function foo() {
  console.log(this);
}
setTimeout(foo, 1);//window
```
按理说我们加了严格模式，foo 调用也没有指定 this，应该是出来 undefined，但是这里仍然出现了全局对象，难道是严格模式失效了吗？并不，即使在严格模式下，setTimeout 方法在调用传入函数的时候，如果这个函数没有指定了的 this，那么它会做一个隐式的操作—-自动地注入全局上下文，等同于调用 foo.apply(window) 而非 foo()；
当然，如果我们在传入函数的时候已经指定 this，那么就不会被注入全局对象，比如： setTimeout(foo.bind(obj), 1);；

* 回调函数里的坑

我们经常在回调函数里面会遇到一些坑：
``` bash
var obj = {
  name: 'qiutc',
  foo: function() {
    console.log(this);
  },
  foo2: function() {
    console.log(this);
    setTimeout(this.foo, 1000);
  }
}

obj.foo2();
```
执行这段代码我们会发现两次打印出来的 this 是不一样的：

第一次是 foo2 中直接打印 this，这里指向 obj 这个对象，我们毋庸置疑；但是在 setTimeout 中执行的 this.foo ，却指向了全局对象，这里不是把它当作函数的方法使用吗？这一点经常让很多初学者疑惑；其实，setTimeout 也只是一个函数而已，函数必然有可能需要参数，我们把 this.foo 当作一个参数传给 setTimeout 这个函数，就像它需要一个 fun参数，在传入参数的时候，其实做了个这样的操作 fun = this.foo，看到没有，这里我们直接把 fun 指向 this.foo 的引用；执行的时候其实是执行了 fun() 所以已经和 obj 无关了，它是被当作普通函数直接调用的，因此 this 指向全局对象。这个问题是很多异步回调函数中普遍会碰到的；

解决方式是通过闭包来完成：
``` bash
var obj = {
  name: 'qiutc',
  foo: function() {
    console.log(this);
  },
  foo2: function() {
    console.log(this);
    var _this = this;
    setTimeout(function() {
      console.log(this);  // Window

      console.log(_this);  // Object {name: "qiutc"}
    }, 1000);
  }
}

obj.foo2();
```
可以看到直接用 this 仍然是 Window；因为 foo2 中的 this 是指向 obj，我们可以先用一个变量_this来储存，然后在回调函数中使用_this，就可以指向当前的这个对象了；
