---
title: javascript函数柯里化详解
date: 2016-10-28 8:50:19
tags: javascript
categories: javascript
---
# 概念
函数柯里化（curry）的定义很简单：传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。
比如对于加法函数 var add = (x, y) =>　x + y ，我们可以这样进行柯里化：
``` bash
//ES5写法
var add = function(x){
  return function(y){
    return x+y;
  }
}
//ES6写法
var add = x => (y => x + y);
var add2 = add(2);  
var add200 = add(200);

add2(2);//4
add2(50);//250
```
与函数绑定紧密相关的主题是函数柯里化(function currying)，它用于创建已经设置好了一个或多个参数的函数。函数柯里化的基本方法和函数绑定是一样的：使用一个闭包返回一个函数。两者的区别在于，当函数被调用时，返回的函数还需要设置一些传入的参数。
柯里化函数通常由以下步骤动态创建：调用另一个函数并为它传入要柯里化的函数和必要参数。下面是创建柯里化函数的通用方式。
``` bash
function curry(fn){
  var args = Array.prototype.slice.call(arguments,1);
  return function(){
    var innerArgs = Array.prototype.slice.call(arguments);
    var finalArgs = args.concat(innerArgs);
    return fn.apply(null,finalArgs);
  }
}

```
柯里化示例：
``` bash
function add(num1,num2) {
  return num1+num2;
}
var curryAdd = curry(add,5);//调用以上柯里化函数
curryAdd(2);//结果7
```
也可以这样：
``` bash
function add(num1,num2) {
  return num1+num2;
}
var curryAdd = curry(add,5，12);//调用以上柯里化函数
curryAdd();//结果17
```

事实上柯里化是一种“预加载”函数的方法，通过传递较少的参数，得到一个已经记住了这些参数的新函数，某种意义上讲，这是一种对参数的“缓存”，是一种非常高效的编写函数的方法：
``` bash
import { curry } from 'lodash';
//首先柯里化两个纯函数
var match = curry((reg,str) => str.match(reg));
var filter = curry((f,str) => str.filter(f));

//判断字符串里面有没有空格
var haveSpace = match(/\s+/g);

haveSpace("ffffff");
//=>null

haveSpace("a b");
//=>[" "]

filter(haveSpace,["abcdefg","hello world"]);
//=>["hello world"]
```
