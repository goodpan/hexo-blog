---
title: js中arguments对象详解
date: 2016-10-18 08:40:53
tags: javascript
categories: javascript
---
## 概念
arguments对象时一个类数组的对象，代表传给一个function的参数列表。
arguments并不是真正的数组，它是一个类数组对象。
## length属性
arguments包含一个length属性，通过arguments.length可获得参数个数。
## argumens转数组
### Array.prototype.slice
通常使用下面的方法来将 arguments 转换成数组：
``` bash
Array.prototype.slice.call(arguments);
```
简写：
[].slice.call(arguments);
在这里，只是简单地调用了空数组的 slice 方法，而没有从 Array 的原型层面调用。
通过借用Array的slice方法可以将类数组（伪数组）转换为数组。slice方法得到的结果是一个数组，参数为arguments。
事实上，满足一定条件的对象都能被slice方法转换为数组。如:
``` bash
const obj = {0:"A",1:"B",2:"C",length:3};
const result = [].slice.call(obj);
console.log(Array.isArray(result), result);
```
结果：
``` bash
true ["A", "B"]
```
通过上面的例子可以发现，满足伪数组的条件：

1) 属性为 0，1，2…；
2） 具有 length 属性；

## arguments 泄露或者传递的问题
不能将函数的 arguments 泄露或者传递出去。什么意思呢？看下面的几个泄露 arguments 的例子：
``` bash
// Leaking arguments example1:
function getArgs(){
    returnarguments;
}

// Leaking arguments example2:
function getArgs(){
    const args = [].slice.call(arguments);
    return args;
}

// Leaking arguments example3:
function getArgs(){
    const args = arguments;
    returnfunction(){
        return args;
    };
}
```
上面的做法就直接将函数的 arguments 对象泄露出去了，最终的结果就是 V8 引擎将会跳过优化，导致相当大的性能损失。

你可以这么做：
``` bash
function getArgs(){
    const args = new Array(arguments.length);
    for(let i = 0; i < args.length; ++i) {
        args[i] = arguments[i];
    }
    return args;
}
```
那就很好奇了，我们每次使用 arguments 时通常第一步都会将其转换为数组，同时 arguments 使用不当还容易导致性能损失，那么为什么不将 arguments 直接设计成数组对象呢？

这需要从这门语言的一开始说起。arguments 在语言的早期就引入了，当时的 Array 对象具有 4 个方法： toString、 join、 reverse 和 sort。arguments 继承于 Object 的很大原因是不需要这四个方法。而现在，Array 添加了很多强大的方法，比如 forEach、map、filter 等等。那为什么现在不在新的版本里让 arguments 重新继承自 Array呢？其实 ES5 的草案中就包含这一点，但为了向前兼容，最终还是被委员会否决了。

## 修改 arguments 值
在严格模式与非严格模式下，修改函数参数值表现的结果不一样。看下面的两个例子：
``` bash
function foo(a){
    "use strict";
    console.log(a, arguments[0]);
    a = 10;
    console.log(a, arguments[0]);
    arguments[0] = 20;
    console.log(a, arguments[0]);
}
foo(1);
```
输出：
``` bash
11
101
1020
```
另一个非严格模式的例子：
``` bash
function foo(a){
    console.log(a, arguments[0]);
    a = 10;
    console.log(a, arguments[0]);
    arguments[0] = 20;
    console.log(a, arguments[0]);
}
foo(1);
```
从上面的两个例子中可以看出，在严格模式下，函数中的参数与 arguments 对象没有联系，修改一个值不会改变另一个值。而在非严格模式下，两个会互相影响。
## 将参数从一个函数传递到另一个函数
下面是将参数从一个函数传递到另一个函数的推荐做法。
``` bash
function foo(){
    bar.apply(this, arguments);
}
functionbar(a, b, c){
    // logic
}
```
注：这种方式经常在框架中使用，通过在构造函数中调用原型的init方法，把参数传递给init方法。
## arguments 与重载
很多语言中都有重载，但 JavaScript 中没有。先看个例子：
``` bash
function add(num1, num2){
    console.log("Method one");
        return num1 + num2;
    }

function add(num1, num2, num3){
    console.log("Method two");
    return num1 + num2 + num3;
}

add(1, 2);
add(1, 2, 3);
```
执行结果为：
``` bash
Method two
Method two
```
所以，JavaScript 中，函数并没有根据参数的不同而产生不同的调用。
是不是 JavaScript 中就没有重载了呢？并不是，我们可以利用 arguments 模拟重载。还是上面的例子。
``` bash
function add(num1, num2, num3){
    if (arguments.length === 2) {
    console.log("Result is " + (num1 + num2));
    }
    elseif (arguments.length === 3) {
        console.log("Result is " + (num1 + num2 + num3));
    }
}

add(1, 2);
add(1, 2, 3)
```
执行结果如下：
``` bash
Result is 3
Result is 6
```
## ES6 中的 arguments
### 扩展操作符
``` bash
function func(){
    console.log(...arguments);
}

func(1, 2, 3);
```
执行结果是：
``` bash
123
```
而不是[1,2,3]
简洁地讲，扩展操作符可以将 arguments 展开成独立的参数。
``` bash
### Rest 参数
function func(firstArg, ...restArgs){
    console.log(Array.isArray(restArgs));
    console.log(firstArg, restArgs);
}

func(1, 2, 3);
```
执行结果是：
``` bash
true
1 [2, 3]
```
从上面的结果可以看出，Rest参数表示除了明确指定外，剩下的参数集合，类型是 Array。
``` bash
### 默认参数
function func(firstArg = 0, secondArg = 1){
    console.log(arguments[0], arguments[1]);
    console.log(firstArg, secondArg);
}
func(99);
```
执行结果是：
``` bash
99undefined
991
```
可见，默认参数对 arguments 没有影响，arguments 还是仅仅表示调用函数时所传入的所有参数。
### es6中 arguments 转数组
Array.from() 是个非常推荐的方法，其可以将所有类数组对象转换成数组。
有时我们会遇到这样的场景，需要创建一个包含从0到99(n)的连续整数的数组。以前我们会这样写:
``` bash
var arr = [];
for(var i = 0; i <= 99; i++) {
    arr.push(i);
}
```
这种方法最直观了，性能也很好。只是不喜欢写 for循环 的同学可能不会这样写，所以有人搞出了下面这种写法:
``` bash
var arr = Array(100).join(' ').split('').map(function(item,index){return index});
```
这种方法中 Array(100) 创建了一个包含100个 undefined 的数组。但是这样的数组是没法迭代的(参考 forEach 方法的 定义 )，所以要通过字符串转换，覆盖 undefined ，最后调用 map 修改元素值。
``` bash
var arr = Array.from({length:100}).map(function(item,index){return index});
```
这种方法中 Array.from({length:100}) 也是创建了一个包含100个 undefined 的数组，Array.from 创建的数组是可以迭代的([].slice.call({length:100})创建的不可迭代)，即使元素值都是 undefined,可以直接调用 map 方法。所以 Array.from 还可以用来实现次数确定的循环遍历。例如在写 React 组件时，有时要 map 迭代确定次数，生成 html。
Array.from 好用归好用，不过在性能上却有些尴尬。上面三种方法第一种性能最好，第二种次之，第三种最差。
## 数组与类数组对象
数组具有一个基本特征：索引。这是一般对象所没有的。
``` bash
const obj = { 0: "a", 1: "b" };
const arr = [ "a", "b" ];
```
我们利用 obj[0]、arr[0] 都能取得自己想要的数据，但取得数据的方式确实不同的。obj[0] 是利用对象的键值对存取数据，而 arr[0] 却是利用数组的索引。事实上，Object 与 Array 的唯一区别就是 Object 的属性是 string，而 Array 的索引是 number。

下面看看类数组对象。
伪数组的特性就是长得像数组，包含一组数据以及拥有一个 length 属性，但是没有任何 Array 的方法。再具体的说，length 属性是个非负整数，上限是 JavaScript 中能精确表达的最大数字；另外，类数组对象的 length 值无法自动改变。

如何自己创建一个类数组对象？
``` bash
function Foo(){}
Foo.prototype = Object.create(Array.prototype);

const foo = new Foo();
foo.push('A');
console.log(foo, foo.length);
console.log("foo is an array? " + Array.isArray(foo));
```
执行结果是：
``` bash
["A"] 1
foo is an array? false
```
也就是说 Foo 的示例拥有 Array 的所有方法，但类型不是 Array。
如果不需要 Array 的所有方法，只需要部分怎么办呢？
``` bash
function Bar(){}
Bar.prototype.push = Array.prototype.push;
const bar = new Bar();
bar.push('A');
bar.push('B');
console.log(bar);
```
执行结果是：
``` bash
Bar {0: "A", 1: "B", length: 2}
```
