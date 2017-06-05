---
title: javascript中String类型详解
date: 2016-10-31 09:13:54
tags: javascript
categories: javascript
---
# 概念

String类型用于表示由零或多个16位Unicode字符组成的字符序列，即字符串。可以由""和''表示，两种写法都是有效的。(与PHP中的双引号和单引号会影响解释方式不同，ECMAscript的这两种语法形式没有区别)。

# 字符串类型的创建

## 字面量方式
``` bash
\n  换行
\t  制表
\b  空格
\r  回车
\f  进纸
\\  斜杠
\'  单引号
\"  双引号
```
这些字符串可以出现在字符串中的任意位置，而且也将作为一个字符来解析。

## String构造函数创建

String类型是字符串的对象包装类型，可以像下面这样使用String构造函数创建：
``` bash
var stringObject = new String("hello world");
```
String对象的方法也可以在所有基本的字符串值中访问到。其中继承的valueOf().toLocalString()和toString()方法，都返回对象所表示的基本字符串值。
String类型的每个实例都有一个length属性，表示字符串中有多个字符。应该注意的是，即使字符串中包含双字节字符（不是占一个字节的ASCII字符），每个字符仍然算一个字符。

# String类型方法

## 字符方法
两个用于访问字符串中特定字符的方法,charAt()和charCodeAt()。这两个方法都接收一个参数，即基于0的字符位置。
1. charAt()
``` bash
var stringValue = "hello world";
alert(stringValue.charAt(1));//"e"
```

2. charCodeAt()
charAt()得到的是字符，如果想得到不是字符而是字符编码，则用charCodeAt()
``` bash
var stringValue = "hello world";
alert(stringValue.charCodeAt(1));//"101"
```
3. 方括号加索引
ECMAscript提供了另一个访问字符串的方法，可以使用方括号加数字索引来访问字符串中的特定字符。
``` bash
var stringValue = "hello world";
alert(stringValue[1]);//"e"
```
这种方式在IE7会返回undefined值。

## 字符串操作方法
1. concat()
将一个或多个字符串拼接起来，返回拼接得到的新字符串。
``` bash
var stringValue = "hello";
var result = stringValue.concat("world");
alert(result);//"hello world"
alert(stringValue);//"hello"
```
concat可以接收任意多个参数，即可以拼接任意多个字符串。
虽然concat可以专门用来拼接字符串，但是实际中使用更多的是用+操作符。

2. slice()


3. substr()

4. subString()

## 字符串位置方法
1. indexOf()

2. lastIndexOf()

## 去空格trim()方法


## 字符串大小写转换方法
1. toLowerCase()


2. toLocaleLowerCase()

3. toUpperCase()

4. toLocaleUpperCase()


## 字符串的模式匹配方法
1. match()

2. search()

3. replace()

## 字符串比较方法localeCompare()

## fromCharCode()方法

## HTML方法




# 字符串特点
字符串是不可变的，一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量：
``` bash
  var lang = "java";
  lang = lang + "script";
```
以上代码后台操作过程为：首先创建一个容纳10个字符的新字符串，然后在这个字符串中填充"java"和"script",最后一步是销毁原来的字符串"java"和"script",因为这两个字符串已经没有用了。

# 字符串类型转换
将一个值转换为字符串有两种方式：
1.toString()
数值、布尔值、对象和字符串（每个字符串也多有一个toString()方法，该方法返回字符串的副本)都有toString()方法。但null和undefined值没有这个方法。
``` bash
var age = 11;
var ageAsString = age.toString();//"11"
var found = true;
var foundAsString = found.toString();//"true"
```
通过给toString()方法传递参数可以输出以二进制、十进制、八进制、十六进制乃至其他任意有效进制格式表示的字符串。
``` bash
var num = 10;
alert(num.toString());//"10"
alert(num.toString(2));//"1010"
alert(num.toString(8));//"12"
alert(num.toString(10));//"10"
alert(num.toString(16));//"a"
```
2.String()转型函数
在不知道要转换的值是不是null或undefined的情况下，可以使用转型函数String()，这个函数能够将任何类型的值转换为字符串。String()函数遵循下列转换规则：

* 如果值有toString()方法，则调用该方法（无参数）并返回相应的结果；
* 如果值是null,则返回"null";
* 如果值是undefined，则返回"undefined"。

``` bash
var value1 = 10;
var value2 = true;
var value3 = null;
var value4;
alert(String(value1));//"10"
alert(String(value2));//"true"
alert(String(value3));//"null"
alert(String(value4));//"undefined"
```

3. +""方式


# 作为数组的字符串
``` bash
var s = "JavaScript";
Array.prototype.join.call(s," ");//=> "J a v a S c r i p t"
Array.prototype.filter.call(s,function(x){
      return x.match(/[^aeiou]/);
  }).join("") //=> "JvScript"
```
