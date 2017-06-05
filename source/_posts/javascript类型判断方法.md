---
title: javascript类型判断方法
date: 2017-01-04 09:49:53
tags: javascript
categories: javascript
---

javascript可以通过typeof、instanceof、constructor、toString.call()、jQuery方法来进行类型判断。
## typeof
typeof 是一元运算符，返回结果是一个说明运算数类型的字符串。用于判断一个变量的类型,常用于区别对象和原始类型。
typeof判断类型时有几个特殊的地方：
* typeof null	结果为Object
* typeof undefined和未定义的变量，结果都是undefined
* typeof	 Date、RegExp、Array实例时均返回Object
由上可看出对象和数组返回都是object,所以typeof不能完美检测。

## instanceof(不常用）
判断一个实例是否属于某种类型；
判断一个实例是否属于它的父类型（继承）;
可以用instanceof运算符来判断对象是否为数组类型。
``` bash
function isArray(arr){
    return arr instanceof Array;
}
var arr = [];//这种方式的本质也是new Array()，构造函数为Array
console.log(isArray(arr));
```
## constructor
每个对象都有constructor属性， 指向初始化该对象的构造函数。常用于判断未知对象的类型检测各种对象类型。
判断数组的函数也可以这样写：
``` bash
function isArray(arr){
    return typeof arr == "object" && arr.constructor == Array;
}
```
## toString.call()
toString是Object的一个方法，返回的结果都不一样。其实是Object.prototype.toString.call(str) ..........'[object  String]'
栗子：判断是否为数组
``` bash
funtion isArray(arr){
    return Object.prototype.toString.call(arr) === '[object Array]';
}
```
## jquery方式
``` bash
jquery.isArray();
jquery.isEmptyObject();
jquery.isFunction();
jquery.isNumeric();
jquery.isPlainObject();
jquery.isPlainOject();
jquery.isWindow();
jquery.isXMLDoc();
```
## 检测各种对象类型
``` bash
var is ={
    types : ["Array", "Boolean", "Date", "Number", "Object", "RegExp", "String", "Window", "HTMLDocument"]
};
for(var i = 0, c; c = is.types[i ++ ]; ){
    is[c] = (function(type){
        return function(obj){
           return Object.prototype.toString.call(obj) == "[object " + type + "]";
        }
    )(c);
}
alert(is.Array([])); // true
alert(is.Date(new Date)); // true
alert(is.RegExp(/reg/ig)); // true
```
