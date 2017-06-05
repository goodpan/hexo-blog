---
title: javascript数组详解(一)
date: 2016-10-24T10:44:19.000Z
tags: javascript
categories: javascript
---

# 基本概念

数组是值的有序集合，每个值叫做一个元素，每个元素在数组中有一个位置，以数字表示，称为索引。 和其他强类型语言不同，在 JavaScript 中，数组是无类型的，可以容纳任何类型的值，可以是字符串、数字、对象（ object ），甚至是其他数组（多维数组就是通过这种方式来实现的）：

```bash
var a = [ 1, "2", [3] ];
a.length; // 3
a[0] === 1; // true
a[2][0] === 3; // true
```

对数组声明后即可向其中加入值，不需要预先设定大小

```bash
var a = [ ];
a.length; // 0
a[0] = 1;
a[1] = "2";
a[2] = [ 3 ];
a.length; // 3
```

使用 delete 运算符可以将单元从数组中删除，但是请注意，单元删除后，数组的 length 属性并不会发生变化。 在创建 " 稀疏 " 数组（ sparse array ，即含有空白或空缺单元的数组）时要特别注意：

```bash
var a = [ ];
a[0] = 1;
//  此处没有设置 a[1] 单元
a[2] = [ 3 ];
a[1]; // undefined
a.length; // 3
```

上面的代码可以正常运行，但其中的 " 空白单元 " （ empty slot ）可能会导致出人意料的结果。 a[1] 的值为 undefined ，但这 与将其显式赋值为 undefined （ a[1] = undefined ）还是有所区别。 数组通过数字进行索引，但有趣的是它们也是对象，所以也可以包含字符串键值和属性（但这些并不计算在数组长度内）：

```bash
var a = [ ];
a[0] = 1;
a["foobar"] = 2;
a.length; // 1
a["foobar"]; // 2
a.foobar; // 2
```

这里有个问题需要特别注意，如果字符串键值能够被强制类型转换为十进制数字的话，它就会被当作数字索引来处理。

```bash
var a = [ ];
a["13"] = 42;
a.length; // 14
```

在数组中加入字符串键值 / 属性并不是一个好主意。建议使用对象来存放键值 / 属性值，用数组来存放数字索引值。

数组继承自Array.prototype中的属性，它定义了一套丰富的数组操作方法。

# 类数组（伪数组）

有时需要将类数组（一组通过数字索引的值）转换为真正的数组，这一般通过数组工具函数（如 indexOf(..) 、 concat(..) 、 forEach(..) 等）来实现。

例如，一些 DOM 查询操作会返回 DOM 元素列表，它们并非真正意义上的数组，但十分类似。另一个例子是通过 arguments 对象（类数组）将函数的参数当作列表来访问（从 ES6 开始已废止）。 工具函数 slice(..) 经常被用于这类转换：

```bash
function foo() {
var arr = Array.prototype.slice.call( arguments );
arr.push( "bam" );
console.log( arr );
}
foo( "bar", "baz" ); // ["bar","baz","bam"]
```

如上所示， slice() 返回参数列表（上例中是一个类数组）的一个数组复本。 用 ES6 中的内置工具函数 Array.from(..) 也能实现同样的功能：

```bash
...
var arr = Array.from( arguments );
```

# 创建数组

## 字面量方式

使用字面量是创建数组的最简单的方法。

```bash
var empty = []; var primes = [1,2,3,4,5]; var misc = [1.1,true,"a"];
```

数组字面量中的值，不一定要是常量，它可以是任意的表达式：

```bash
var base = 1024; var table = [base,base+1,base+2,base+3];
```

它可以包含对象直接量或其他数组直接量：

```bash
var b = [[1,{x:1,y:2}],[2,{x:3,y:4}]];
```
如果省略数组直接量中的某个值，省略的元素将被赋予undefined值：
```bash
var count = [1,,3];//数组有3个元素，中间元素值为undefined
var undefs = [,,];//数组有2个元素，都是undefined
```
因为数组直接量语法允许有可选的结尾的逗号，所以[,,]只有2个元素而非3个。
## 调用构造函数Array()创建数组
* 调用时没有参数
```bash
var arr = new Array();
```
该方法创建一个没有任何元素的空数组，等同于数组字面量[]。
* 调用时传入 数值参数，指定数组长度
```bash
var arr = new Array(10);
```
该方式创建指定长度的数组。当预先知道所需元素个数时，这种形式的Array()构造函数可以用来预先分配一个数组空间。注意，数组中没有存储值，甚至数组的索引"0"、"1"等还未定义。
* 显示指定2个或者多个数组元素或数组的一个非数值元素：
```bash
var arr = new Array(5,4,3,2,1,"testing,testing");
```
以这种形式，构造函数的参数将会成为新数组的元素。使用数组字面量比这样使用Array()构造函数简单多了。

# 稀疏数组
稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中元素的个数。如果数组是稀疏的，length属性值大于元素的个数。可以用Array()构造函数或简单地指定数组的索引值大于当前的数组长度来创建稀疏数组。
```bash
var arr = new Array(5);//数组没有元素，但是a.length是5。
arr = [];//创建一个空数组，length = 0;
arr[1000] = 0;//赋值添加一个元素，但是设置length为1001；
```
足够稀疏的数组实际上比稠密的数组更慢，内存利用率更高，在这样的数组中查找元素的时间与常规对象属性的查找时间一样。
注意：当在数组字面量中省略值时不会创建稀疏数组。省略元素在数组中是存在的，其值为undefined。这和数组元素根本不存在是有一些微妙的区别的。可以用in操作符检测两者之间的区别。
```bash
var a1 = [,,,];//数组是[undefinded,undefinded,undefinded];
var a2 = new Array(3);//该数组根本没有元素
0 in a1     //=> true:a1 在索引0处有一个元素。
0 in a2     //=> false:a2 在索引0处没有元素
```
在一些旧版本的浏览器中，在存在连续逗号的情况下，插入undefined值的操作则与此不同，在这些实现中，[1,,3]和[1,undefinde,3]是一模一样的。
# length属性
每个数组都有一个length属性，就是这个属性使其区别于常规的javascript对象。稠密数组的length值比数组最大索引大1。
而稀疏数组的length属性则大于元素的个数。
当设置length属性为一个小于当前长度的非负整数n时，当前数组中那些索引值大于或等于n的元素将从中删除。
```bash
var a = [1,2,3,4,5];//从5个元素的数组开始
a.length = 3;//现在a为[1,2,3]
a.length = 5;//长度为5，但是没有元素，就像new Array(5);
```
当设置length属性为一个大于当前长度的非负整数n时，实际上这不会向数组中添加新的元素，它只是在数组尾部创建一个空的区域。
```bash
var a = [1,2,3];
Object.defineProperty(a,"length",{writable,false});
a.length = 0;
```
在ECMAScript 5中，可以用Object.defineProperty()让数组的length属性变成只读的。
