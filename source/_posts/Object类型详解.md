---
title: Object类型详解
date: 2016-11-01 09:15:29
tags: javascript
categories: javascript
---
# 引用类型
引用类型的值（对象）是引用类型的一个实例。在ECMAScript中，引用类型是一种数据结构，用于将数据和功能组织在一起。引用类型有时候也被成为对象定义，因为它们描述的是一类对象所具有的属性和方法。

对象是某个特定引用类型的实例。新对象是使用new操作符后跟一个构造函数来创建的。构造函数本身就是一个函数，只不过该函数是出于创建新对象的目的而定义的。
``` bash
var person = new Object();
```
这行代码创建了一个Object新实例，然后把该实例保存在了变量person中。使用的构造函数式Object，它只为新对象定义了默认的属性和方法。ECMAScript提供了很多原生引用类型（例如Object)，以便开发人员用以实现常见的计算任务。

# Object实例的创建
1. new操作符方式
``` bash
var person = new Object();
person.name = "Nicholas";
person.age = 29;
```
2. 对象字面量方式
对象字面量是对象定义的一个简写形式，目的在于简化创建包含大量属性的对象的过程。
``` bash
var person = {
  name:"Nicholas",
  age:29
}
```
在对象字面量语法中，属性名也可以使用字符串。
使用对象字面量语法，可以通过'.'的方式访问对象属性，也可以使用"[属性名]"来访访问属性。从功能上看，这二者没有区别。但"[]"的优点是可以通过变量来访问属性：
``` bash
var propertyName = "name";
alert(person[propertyName]);//"Nicholas"
```
如果属性名中包含会导致语法错误的字符，或者属性名使用的是关键字或者保留字，也可以使用方括号表示法。

# Object属性

## Constructor
保存用于创建当前对象的函数。

# Object方法

## Object.assign()
Object.assign函数是ES2015的内容，使用该函数我们可以快速的复制一个或者多个对象到目标对象中
``` bash
Object.assign(target, ...sources)
```
### 函数原型

函数参数为一个目标对象（该对象作为最终的返回值）,源对象(此处可以为任意多个)。通过调用该函数可以拷贝所有可被枚举的自有属性值到目标对象中。
这里我们需要强调的三点是：

1. 可被枚举的属性
2. 自有属性
3. string或者Symbol类型是可以被直接分配的
拷贝过程中将调用源对象的getter方法，并在target对象上使用setter方法实现目标对象的拷贝。

### 函数实例
``` bash
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.
```
最开始的o1因为设置为target，则调用其setter方法设置了其他对象的属性到自身。
``` bash
var obj = Object.create({ foo: 1 }, { // foo is an inherit property.
  bar: {
    value: 2  // bar is a non-enumerable property.
  },
  baz: {
    value: 3,
    enumerable: true  // baz is an own enumerable property.
  }
});

var copy = Object.assign({}, obj);
console.log(copy); // { baz: 3 }  
```
我们自定义了一些对象，这些对象有一些包含了不可枚举的属性,另外注意使用 Object.defineProperty 初始化的对象默认是不可枚举的属性。对于可枚举的对象我们可以直接使用Object.keys()获得,或者使用for-in循环遍历出来.
对于不可枚举的属性，使用Object.assign的时候将被自动忽略。
``` bash
var target = Object.defineProperty({}, 'foo', {
  value: 1,
  writable: false
});

Object.assign(target, { bar: 2 })

//{bar: 2, foo: 1}

Object.assign(target, { foo: 2 })
//Uncaught TypeError: Cannot assign to read only property 'foo' of object '#<Object>'(…)
```
对于只读的属性，当分配新的对象覆盖他的时候，将抛出异常。

### Polyfill

1. 实现es5版本的Object.assign：
2. 判断是否原生支持该函数，如果不存在的话创建一个立即执行函数，该函数将创建一个assign函数绑定到Object上。
3. 判断参数是否正确(目的对象不能为空，我们可以直接设置{}传递进去,但必须设置该值)
4. 使用Object在原有的对象基础上返回该对象，并保存为out
5. 使用for…in循环遍历出所有的可枚举的自有对象。并复制给新的目标对象(hasOwnProperty返回非原型链上的属性)
``` bash
if (typeof Object.assign != 'function') {
  (function () {
	Object.assign = function (target) {
	 'use strict';
	 if (target === undefined || target === null) {
	   throw new TypeError('Cannot convert undefined or null to object');
	 }

	 var output = Object(target);
	 for (var index = 1; index < arguments.length; index++) {
	   var source = arguments[index];
	   if (source !== undefined && source !== null) {
	     for (var nextKey in source) {
	       if (source.hasOwnProperty(nextKey)) {
	         output[nextKey] = source[nextKey];
	       }
	     }
	   }
	 }
	 return output;
	};
})();
}
```
## Object.create()
创建一个具有指定原型且可选择性地包含指定属性的对象
``` bash
Object.create(prototype, descriptors)
```
参数:
prototype 必需。  要用作原型的对象。 可以为 null。
descriptors 可选。 包含一个或多个属性描述符的 JavaScript 对象。

简单来讲，new Object()是一种通过构造函数来创建object的方式，而Object.create(proto, [ propertiesObject ])
不需要通过构造函数就可以创建一个object，Object.create()的第一个参数是必须要的，第二个参数可选。
## Object.defineProperties()
将一个或多个属性添加到对象，并/或修改现有属性的特性。
``` bash
object.defineProperties(object, descriptors)
```
参数
object
必需。  对其添加或修改属性的对象。  这可以是本机 JavaScript 对象或 DOM 对象。  
descriptors
必需。  包含一个或多个描述符对象的 JavaScript 对象。  每个描述符对象描述一个数据属性或访问器属性。  

返回值
已传递给函数的对象。

备注
descriptors 参数是一个包含一个或多个描述符对象的对象。
数据属性是可储存和检索值的属性。  数据属性描述符包含一个 value 特性和/或一个 writable 特性。  有关更多信息，请参见数据属性和访问器属性。  
一旦设置或检索属性值，访问器属性就会调用用户提供的函数。  
访问器属性描述符包含 set 特性和/或 get 特性。  
如果对象已包含一个带指定名称的属性，则修改该属性的特性。  
若要创建一个对象并向新对象添加属性，则可使用 Object.create 函数 (JavaScript)。
``` bash
var newLine = "<br />";

var obj = {};
Object.defineProperties(obj, {
    newDataProperty: {
        value: 101,
        writable: true,
        enumerable: true,
        configurable: true
    },
    newAccessorProperty: {
        set: function (x) {
            document.write("in property set accessor" + newLine);
            this.newaccpropvalue = x;
        },
        get: function () {
            document.write("in property get accessor" + newLine);
            return this.newaccpropvalue;
        },
        enumerable: true,
        configurable: true
    }});

// Set the accessor property value.
obj.newAccessorProperty = 10;
document.write ("newAccessorProperty value: " + obj.newAccessorProperty + newLine);

// Output:
// in property set accessor
// in property get accessor
// newAccessorProperty value: 10
```
## Object.defineProperty()
将属性添加到对象，或修改现有属性的特性。
``` bash
Object.defineProperty(object, propertyname, descriptor)
```
参数
object
必需。  要在其上添加或修改属性的对象。  这可能是一个本机 JavaScript 对象（即用户定义的对象或内置对象）或 DOM 对象。  
propertyname
必需。  一个包含属性名称的字符串。  
descriptor
必需。  属性描述符。  它可以针对数据属性或访问器属性。  

返回值
已修改对象。

备注
可使用 Object.defineProperty 函数来执行以下操作：
向对象添加新属性。  当对象不具有指定的属性名称时，发生此操作。  
修改现有属性的特性。  当对象已具有指定的属性名称时，发成此操作。  
描述符对象中会提供属性定义，用于描述数据属性或访问器属性的特性。  描述符对象是 Object.defineProperty 函数的参数。  
若要向对象添加多个属性或修改多个现有属性，可使用 Object.defineProperties 函数 (JavaScript)。
``` bash
var newLine = "<br />";

// Create a user-defined object.
var obj = {};

// Add a data property to the object.
Object.defineProperty(obj, "newDataProperty", {
    value: 101,
    writable: true,
    enumerable: true,
    configurable: true
});

// Set the property value.
obj.newDataProperty = 102;
document.write("Property value: " + obj.newDataProperty + newLine);

// Output:
// Property value: 102
```
## Object.freeze()
阻止修改现有属性的特性和值，并阻止添加新属性。
``` bash
Object.freeze(object)
```
参数
object
必需。在其上锁定特性的对象。
返回值
传递给函数的对象。

## getOwnPropertyDescriptor()
获取指定对象的自身属性描述符。自身属性描述符是指直接在对象上定义（而非从对象的原型继承）的描述符。
``` bash
Object.getOwnPropertyDescriptor(object, propertyname)
```
参数
object
必需。包含属性的对象。
propertyname
必需。属性的名称。

返回值
属性的描述符。
``` bash
// Create a user-defined object.
var obj = {};

// Add a data property.
obj.newDataProperty = "abc";

// Get the property descriptor.
var descriptor = Object.getOwnPropertyDescriptor(obj, "newDataProperty");

// Change a property attribute.
descriptor.writable = false;
Object.defineProperty(obj, "newDataProperty", descriptor);
```
## getOwnPropertyDescriptors()

Object.getOwnPropertyDescriptors(obj)

## getOwnPropertyNames()
返回对象自己的属性的名称。一个对象的自己的属性是指直接对该对象定义的属性，而不是从该对象的原型继承的属性。对象的属性包括字段（对象）和函数。
``` bash
Object.getOwnPropertyNames(object)
```
参数
Object
必需。包含自己的属性的对象

返回值
一个数组，其中包含对象自己的属性的名称。
``` bash
function Pasta(grain, width, shape) {
    // Define properties.
    this.grain = grain;
    this.width = width;
    this.shape = shape;
    this.toString = function () {
        return (this.grain + ", " + this.width + ", " + this.shape);
    }
}

// Create an object.
var spaghetti = new Pasta("wheat", 0.2, "circle");

// Get the own property names.
var arr = Object.getOwnPropertyNames(spaghetti);
document.write (arr);

// Output:
//   grain,width,shape,toString
```
## getOwnPropertySymbols()

## getPrototypeOf()

## is()
返回一个值，该值指示两个值是否相同。

``` bash
Object.is(value1, value2)
```
参数
value1
必需。要测试的第一个值。
value2
必需。要测试的第二个值。

返回值
如果值相同，则为 true；否则为 false。

备注
与 = = 运算符不同，Object.is 在测试值时不会强制任何类型。
Object.is 应用的比较类似于 === 运算符所应用的比较，区别在于 Object.is 将 Number.isNaN 视作与 NaN 相同的值。它还将 + 0 和-0 视作不同值。
## isExtensible()

## isFrozen()

## isSealed()

## keys()

## preventExtensions()

## hasOwnProperty()


## isPrototypeOf()

## propertyIsEnumerable()

## toLocaleString()

## toString()

## valueOf()

## watch()

## seal()

## setPrototypeOf()
