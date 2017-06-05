---
title: javascript数组详解（二）
date: 2016-10-25 08:35:11
tags: javascript
categories: javascript
---
# 数组元素的添加和删除
## 添加元素
* 为新索引赋值
``` bash
var a = [];//开始一个空数组
a[0]="zero";//然后向其中添加元素
a[1]="one";
```
* push()方法
可以使用push方法在数组末尾增加一个或多个元素
``` bash
var a = [];
a.push("zero");//a=["zero"];
a.push("one","two");//a=["zero","one","two"];
```
* a[a.length]
在数组尾部压入一个元素与给数组a[a.length]赋值是一样的。
* unshift()方法
向数组的首部插入一个元素，并且将其他元素依次移到更高的索引处。
## 删除元素
* delete运算符
可以像删除对象属性一样使用delete运算符来删除数组元素。
``` bash
var a = [1,2,3];
delete a[1];//a在索引1的位置不再有元素
1 in a  //=> false:数组索引1并未在数组中定义。
```
删除数组元素与为其赋值undefined是一样的（但有一些微妙的区别）。
注意：对一个数组元素delete不会修改数组的length属性，也不会将数组元素从高索引处移下来填充已删除属性留下的空白。
如果从数组中删除一个元素，它就变成稀疏数组。
* 给length属性赋值删除数组元素
可以简单地给数组属性length赋一个期望的长度来删除数组尾部的元素。
* pop()方法
从尾部移除数组元素，一次使减少长度1并返回被删除元素的值。
* shift()方法
从数组头部删除一个元素。和delete不同的是，shift()方法将所有元素下移到比当前索引低1的地方。
## splice()方法
splice()方法是一个通用方法，可以插入、删除或替换数组元素。它会根据需要修改length属性并移动元素到更高或较低的索引处。
# 数组遍历
## for循环遍历
for循环遍历是最常见的方法：
``` bash
var keys = Object.keys(o);//获得o对象属性名组成的数组。
var values = [];//在数组中存储匹配属性的值。
for(var i = 0;i < keys.length;i++){
  var key = key[i];
  values[i] = o[key];
}
```
在嵌套循环或其他性能非常重要的上下文中，可以看到这种基本的数组遍历需要优化，数组的长度应该只查询一次而非每次循环都要查询：
``` bash
for(var i = 0,len = keys.length;i<len;i++){
  var key = key[i];
  values[i] = o[key];
}
```
排除null,undefined和不存在的元素：
``` bash
for(var i = 0;i<a.length;i++){
  if(!a[i]) continue;//跳过null、undefined和不存在的元素
}
```
跳过undefined和不存在的元素：
``` bash
for(var i = 0;i<a.length;i++){
  if(a[i] === undefined) continue;//跳过null、undefined和不存在的元素
}
```
跳过不存在的元素而仍然要处理存在undefined元素：
``` bash
for(var i = 0;i<a.length;i++){
  if(!(i in a)) continue;//跳过null、undefined和不存在的元素
}
```
## for in循环
``` bash
for(var index in sparseArray){
  var value = sparseArray[index];
}
```
for/in循环能够枚举继承的属性名，如添加到Array.prototype中的方法。由于这个原因，在数组上不应该使用for/in循环，除非使用额外的检测方法来过滤不想要的属性。如下检测代码取其一即可：
``` bash
for(var i in a){
  if(!a.hasOwnProperty(i)) continue;//跳过继承的属性
}
for(var i in a){
  //跳过不是非负整数的i
  if(String(Math.floor(Math.abs(Number(i))))!==i) continue;
}
```
ECMAScript规范允许for/in循环以不同的顺序遍历对象的属性。通常数组元素的遍历实现是升序的，但不能保证一定是这样的。特别地，如果数组同时拥有对象属性和数组元素，返回的属性名很可能是按照创建的顺序而非数组的大小顺序。如何处理这个问题的实现各不相同，如果算法依赖于遍历的顺序，那么最好不要用for/in而用常规的for循环。
## forEach方法
ECMAScript5定义了一些遍历数组元素的新方法，按照索引的顺序按个传递给定义的一个函数。这些方法中最常用的就是forEach方法：
``` bash
var arr = [1,2,3,4,5];
var sumOfSquares = 0;//要得到数据的平方和
arr.forEach(function(x){//把每个元素传递给此函数
  sumOfSquares+=x*x;//平方相加
})
sumOfSquares
```
forEach()和相关的遍历方法使得数组拥有简单而强大的函数式编程风格。
# ECMAScript3数组的方法
在Array.prototype中定义了一些有用的操作数组的函数，这意味着这些函数作为任何数组的方法都是可用的。
## join()方法
Array.join()将数组中所有元素转化为字符串并连接在一起，返回最后生成的字符串。可以指定分隔符来分隔数组元素，如果不指定则默认用逗号。
``` bash
var arr = [1,2,3];
arr.join();//"1,2,3"
arr.join(" ");//"1 2 3"
arr.join("");//"123"
var b = new Array(10);
b.join("-");//=> "---------"，结果为9个连字号组成的字符串
```
split()方法是Array.join()的逆向操作，用来将字符串分隔成若干块来创建一个数组
## reverce()方法
Array.reverce()方法将数组中的元素颠倒顺序。返回逆序的数组。它采取了替换；换句话说，它不通过重新排列的元素创建新的数组，而是在原先数组中重新排序。
``` bash
var arr = [1,2,3];
arr.reverce().join();//=>"3,2,1",并且现在的arr是[3,2,1]
```
## sort()方法
Array.sort()方法将数组中的元素排序并返回排序后的数组。
* 不带参数，按字母表排序
当不带参数调用sort()的时候，数组元素以字母表顺序排序（如有必要将临时转化为字符串进行比较）。
``` bash
var a = new Array("banana","cherry","apple");
a.sort();
var s = a.join(", ");//s=="apple,banana,cherry"
```
* 包含undefined

如果数组包含undefined元素，它们会被排到数组的尾部。
* 传递函数，按非字母表排序
为了按照其他方式而非字母表排序进行数组排序，必须给sort()方法传递一个比较函数。该函数决定了它的两个参数在排好序的数组中的先后顺序。假设第一个参数应该在前，那么比较函数应该返回一个小于0的数值。反之，假设第一个参数应该在后，那么函数返回一个大于0的数值。如果两个数相等，函数应该返回0。
``` bash
var a = [33,4,1111,222];
a.sort();//字母排序：1111,222,33,4
a.sort(function(a,b){//数值顺序：4,33,222,111
  return a-b;//根据顺序，返回负数、0、整数
})
a.sort(function(a,b){//数值大小相反的排序
  return b-a;
})
```
* 按不区分大小写的字母排序
如果要对一个字符串数组进行不区分大小写排序，那么传递的比较函数首先要将参数转化为小写字符串（使用toLowerCase()方法），然后再进行比较:
``` bash
var arr = ['ant','Bug','cat','Dog'];
arr.sort();//区分大小写的排序["Bug", "Dog", "ant", "cat"]
arr.sort(function(s,t){
    var a = s.toLowerCase();
    var b = t.toLowerCase();
    if(a<b) return -1;
    if(b<a) return 1;
    return 0;
});//=> ["ant", "Bug", "cat", "Dog"]
```
## concat()方法
Array.concat()方法创建并返回一个新数组，它的元素包括调用concat()的原始数组的元素和concat()的每个元素。如果这些参数中的任何一个自身是数组，则连接的是数组的元素，而非数组本身。但要注意，concat()不会递归扁平化数组的数组。concat()也不会修改调用的数组。
``` bash
var arr = [1,2,3];
arr.concat(4,5);//[1, 2, 3, 4, 5]
arr.concat([4,5]);//[1, 2, 3, 4, 5]
arr.concat([4,5],[6,7]);//[1, 2, 3, 4, 5]
arr.concat(4,[5,[6,7]]);//[1, 2, 3, 4, 5,[6,7]]
```
## slice()方法
Array.slice()方法返回指定数组的一个片段或子数组。它的两个参数分别指定了片段的开始和结束的位置。
* 无参数
返回原数组
``` bash
var arr = [1,2,3,4,5];arr.slice()
[1, 2, 3, 4, 5]
```
* 一个参数
如果只指定一个参数，返回的数组将包含从开始位置到数组结尾的所有元素。
``` bash
var arr = [1,2,3,4,5];
arr.slice(2);
[3, 4, 5]
```
* 两个参数
返回的数组包含第一个参数指定的位置和所有到但不含第二个参数指定的位置之间的所有数组元素。
如参数中出现负数，它表示相对于数组中最后一个元素的位置。例如，参数-1指定了最后一个元素，而-3指定了倒数第三个元素。
``` bash
var arr = [1,2,3,4,5];
arr.slice(0,3);//[1,2,3]
arr.slice(3);//[4,5]
arr.slice(1,-1);//[2,3,4]
arr.slice(-3,-2);//[3]
注意：slice()不会修改调用的数组。
``` bash
var arr = [1,2,3,4,5];
arr.slice(2);
[3, 4, 5]
arr
[1, 2, 3, 4, 5]//数组不变
```
## splice()方法
Array.splice()是在数组中插入或者删除元素的通用方法。
不同于slice()、concat(),splice()会修改调用的数组。
splice()能够从数组中删除元素、插入元素到数组或者同时完成这两种操作。在插入或删除点之后的数组元素会根据需要增加或减少它们的索引值，因此数组的其他部分仍然保持连续的。
splice()的第一个参数指定了插入和（或）删除的起始位置。第二个参数指定了应该从数组中删除的元素的个数。如果省略第二个参数，从起始点开始到数组结尾的所有元素都将被删除。splice()返回一个由删除元素组成的数组，或者如果没有删除元素就返回一个空数组。
``` bash
var a = [1,2,3,4,5,6,7,8];
a.splice(4);//返回[5, 6, 7, 8]，a是[1, 2, 3, 4]
a.splice(1,2);//[2,3],a是[1, 4, 5, 6, 7, 8]
a.splice(1,1);//[4],a是[1, 5, 6, 7, 8]
```
splice()的前2个参数指定了需要删除的数组元素。紧随其后的任意个数的参数指定了需要插入到数组的元素，从第一个参数指定的位置开始插入：
``` bash
var a = [1,2,3,4,5];
a.splice(2,0,'a','b');//返回[],a是[1, 2, "a", "b", 3, 4, 5]
a.splice(2,2,[1,2],3);//返回["a", "b"]，a是[1, 2, [1,2], 3, 3, 4, 5]
```
注意：区别于slice()、concat(),splice()会插入数组本身而非数组的元素。
## push()和pop()方法
push()和pop()方法允许将数组当做栈来使用。push()方法在数组的末尾添加一个或多个元素，并返回数组新的长度。pop()方法则相反：
它删除数组的最后一个元素，减少数组长度并返回它删除的值。
注意：两个方法都修改并替换原始数组而非生成一个修改版的新数组。
组合使用push()和pop()可以实现js数组的先进后出的栈。
``` bash
var stack = [];
stack.push(1,2);
stack.pop();
stack.push(3);
stack.pop();
stack.push([4,5]);
stack.pop();
stack.pop();
```
## unshift()和shift()方法
unshift()和shift()方法非常类似于push()和pop()方法，不一样的是前者是在数组的头部而非尾部进行元素的插入和删除操作。unshift()在数组的头部添加一个或多个元素，并将已存在的元素移动到更高索引的位置来获得足够的空间，最后返回数组新的长度。shift()删除数组的第一个元素并将其返回，然后把所有随后的元素下移一个位置来填补数组头部的空缺。
``` bash
var a = [];
a.unshift(1);//a:[1] 返回1
a.unshift(22);//a:[22,1] 返回2
a.shift();//a:[1]  返回22
a.unshift(3,[4,5]);//a:[3,[4,5],1]  返回3
a.shift();  //a:[[4,5],1] 返回3
a.shift();  //a:[1] 返回[4,5]
a.shift();  //a:[]  返回1
```
注意：当使用多个参数调用unshift()时它的行为令人惊讶。参数是一次性插入的(就像splice()方法)而非一次一个地插入。这意味着最终的数组插入的元素的顺序和它们在参数列表的顺序一致。而假如元素是一次一个地插入，它们的顺序应该是反过来。
## toString()和toLocaleString()方法
数组和其他的javascript对象一样拥有toString()方法。针对数组，该方法将其每个元素转化为字符串(如有必要将调用元素的toString()方法)并且输出用逗号分隔的字符串列表。
注意：输出不包括方括号或其他任何形式的包裹数组值的分隔号：
``` bash
[1,2,3].toString();//生成'1,2,3'
["a","b","c"].toString();//生成'a,b,c'
[1,[2,'c']].toString();//生成'1,2,c'
```
注意：这里与不使用任何参数调用join()方法返回的字符串是一样的。
toLocaleString()是toString()方法的本地化版本。它调用元素的toLocaleString()方法将每个数组元素转化为字符串，并且使用本地化（和自定义实现的）分隔符将这些字符串连接起来生成最终的字符串。
# ECMAScript5中的数组方法
ECMAScript5定义了9个新的数组方法来遍历、映射、过滤、检测、简化和检索数组。

## forEach()方法
forEach()方法从头至尾遍历数组，为每个数组元素调用指定的函数。传递的函数作为forEach()的第一个参数。然后forEach()使用第三个参数调用该函数：数组元素、元素的索引和数组本身。如果只关心数组元素的值，可以编写只有一个参数的函数---额外的参数将被忽略：
``` bash
var data = [1,2,3,4,5];   //要求和的数组
//计算数组元素的和值
var sum = 0;
data.forEach(function (value) {
    sum+=value;
})//  sum = 15
//每个数组元素的值自加1
data.forEach(function (v,i,a) {
  a[i] = v+1;
})//  => [2,3,4,5,6]
```
注意：forEach()中不能用break语句终止遍历。如果要提前终止遍历，必须要把forEach()方法放在一个try块中，并能抛出一个异常。如果forEach()调用的函数抛出foreach.break异常，循环会提前终止:
``` bash
function foreach(a,f,t){
  try{}
  catch(e){
    if(e===foreach.break)return;
    else throw e;
  }
}
foreach.break = new Error("StopIteration")
```
## map()方法
map()方法将调用的数组的每个元素传递给指定的函数，并返回一个数组，它包含该函数的返回值。如：
``` bash
a=[1,2,3];
b=a.map(function(x){return x*x;});  //b是[1,4,9]
```
传递给map()的调用方式和传递给forEach()的调用方式一致。但传递给map()的函数应该有返回值。注意,map()返回的是数组：它不修改调用的数组。如果是稀疏数组，返回的也是相同方式的稀疏数组：它具有相同的长度，相同的缺失函数。

## filter()方法
filter()方法返回的数组元素是调用的数组的一个子集。传递的函数是用来逻辑判定的：该函数返回true或false。调用判定函数就像调用forEach()和map()一样。如果返回值为true或能转化为true的值，那么传递给判定函数的元素就是这个子集的成员，它将被添加到一个作为返回值的数组中。
``` bash
var a = [1,2,3,4,5];
smallvalues = a.filter(function(x){return x<3}); //[2,1]
everother = a.filter(function(x,i){return i%2==0}); //[1, 3, 5]
```
## every()和some()方法
every()和some()方法是数组的逻辑判断:它们对数组元素应用指定的函数进行判定，返回true和false。
every()方法就像数学中“针对所有”的量词，当前仅当针对数组中的所有元素调用判定函数都返回true，它才返回true:
``` bash
var a = [1,2,3,4,5];
a.every(function(x){return x<10});  //=>true:所有的值是<10
a.every(function(x){return x%2==0}); //=>false:不是所有的值都是偶数
```
some()方法就像数学中的“存在”量词：当数组中至少有一个元素调用判定函数返回true，它就返回true；并且当且仅当数值中的所有元素调用判定函数都返回false，它才返回false。
``` bash
var a = [1,2,3,4,5];
a.some(function(x){return x%2 ===0;});  //=>true
a.some(isNaN);  //=>false a不包含非数值元素
```
注意：一旦every()和some()确认该返回什么值它们就会停止遍历数组元素。
根据数学上的惯例，在空数组上调用时，every()返回true，some()返回false
``` bash
var a = [];a.some(function(){return});
false
var a = [];a.every(function(){return});
true
```
## reduce()和reduceRight()方法
reduce()和reduceRight()方法使用指定的函数将数组元素进行组合，生成单个值。这个在函数式编程中是常见的操作，也可以称作“注入”和“折叠”。
``` bash
var a = [1,2,3,4,5];
var sum = a.reduce(function(x,y){return x+y},0);  //数组求和
var product = a.reduce(function(x,y){return x*y},1);  //数组求积
var max = a.reduce(function(x,y){return (x>y)?x:y;}); //求最大值
```
## indexOf()和lastIndexOf()方法
indexOf()和lastIndexOf()方法搜索整个数组中具有给定值的元素，返回找到的第一个元素的索引或者如果没有找到返回-1。indexOf()从头到尾搜索，而lastIndexOf()则反向搜索。
``` bash
var a = [0,1,2,1,0];
a.indexOf(1); // =>1:a[1]是1
a.lastIndexOf(1); //=>3:a[3]是1
a.indexOf(3); // =>-1：没有值为3的元素
```
