---
title: flexbox兼容写法（包括微信）
date: 2017-01-03 11:12:34
tags: css
---

## 容器display
``` bash
/*块级*/
.box{
     display:-webkit-box;/* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
    display:-moz-box;/* 老版本语法: Firefox (buggy) */
    display:-ms-flexbox;/* 混合版本语法: IE 10 */
    display:-webkit-flex;/* 新版本语法: Chrome 21+ */
    display: flex;/* 新版本语法: Opera 12.1, Firefox 22+ */
    display: box;/*wexin x5*/
}
```
``` bash
/*精简去注释*/
.box{
     display:-webkit-box;
    display:-moz-box;
    display:-ms-flexbox;
    display:-webkit-flex;
    display: flex;
    display: box;
}
```
``` bash
/*行内*/
.box{
    display: inline-flex;
}
```
## 子元素的显示方向
``` bash
/*从左到到右*/
.box{
    -webkit-box-direction: normal;
    -webkit-box-orient: horizontal;
    -moz-flex-direction: row;
    -webkit-flex-direction: row;
    flex-direction: row;
}
```
``` bash
/*从右到左*/
.box{
    -webkit-box-pack:end;
    -webkit-box-direction: reverse;
    -webkit-box-orient: horizontal;
    -moz-flex-direction: row-reverse;
    -webkit-flex-direction: row-reverse;
    flex-direction: row-reverse;
}
```
``` bash
/*从上到下*/
.box{
    -webkit-box-direction: normal;
    -webkit-box-orient: vertical;
    -moz-flex-direction: column;
    -webkit-flex-direction: column;
    flex-direction: column;
}
```
``` bash
/*从下到上*/
.box{
    -webkit-box-pack:end;
    -webkit-box-direction: reverse;
    -webkit-box-orient: vertical;
    -moz-flex-direction: column-reverse;
    -webkit-flex-direction: column-reverse;
    flex-direction: column-reverse;
}
```
## 子元素主轴对齐方式
``` bash
/*总*/
.box{
    -webkit-box-pack: flex-start | flex-end| center | space-between | space-around;
    -moz-justify-content: flex-start | flex-end| center | space-between | space-around;
    -webkit-justify-content: flex-start | flex-end| center | space-between | space-around;
    justify-content: flex-start | flex-end| center | space-between | space-around;
    box-pack: start |end| center | justify;
}
```
``` bash
/*左*/
.box{
    -webkit-box-pack: flex-start;
    -moz-justify-content: flex-start;
    -webkit-justify-content: flex-start;
    justify-content: flex-start;
    box-pack: start;
}
```
``` bash
/*右*/
.box{
    -webkit-box-pack: center ;
    -moz-justify-content: center ;
    -webkit-justify-content: center;
    justify-content:center;
    box-pack: center;
}
```
``` bash
/*中*/
.box{
    -webkit-box-pack: space-between;
    -moz-justify-content: space-between;
    -webkit-justify-content: space-between;
    justify-content:space-between;
    box-pack: justify;
}
```
``` bash
/*左右*/
.box{
    -webkit-box-pack: flex-end;
    -moz-justify-content: flex-end;
    -webkit-justify-content: flex-end;
    justify-content: flex-end;
}
```
``` bash
/*留空白*/
.box{
    -webkit-box-pack: space-around;
    -moz-justify-content: space-around;
    -webkit-justify-content: space-around;
    justify-content: space-around;
    box-pack: space-around;
}
```
## 子元素交叉轴对齐方式
``` bash
/*总*/
.box{
    -webkit-box-align: flex-start | flex-end| center | baseline | stretch;
    -moz-align-items: flex-start | flex-end| center | baseline | stretch;
    -webkit-align-items: flex-start | flex-end| center | baseline | stretch;
    align-items: flex-start | flex-end| center | baseline | stretch;
    box-align: start |end| center | baseline | stretch;
}
```
``` bash
/*上*/
.box{
    -webkit-box-align: flex-start;
    -moz-align-items: flex-start;
    -webkit-align-items: flex-start;
    align-items: flex-start;
    box-align: start;
}
```
``` bash
/*下*/
.box{
    -webkit-box-align: center;
    -moz-align-items:center ;
    -webkit-align-items:center ;
    align-items:center;
    box-align: center;
}
```
``` bash
/*中*/
.box{
    -webkit-box-align: flex-end;
    -moz-align-items: flex-end;
    -webkit-align-items: flex-end;
    align-items: flex-end;
    box-align: end;
}
```
``` bash
/*基线*/
.box{
    -webkit-box-align: baseline ;
    -moz-align-items: baseline ;
    -webkit-align-items: baseline ;
    align-items: baseline ;
    box-align: baseline ;
}
```
``` bash
/*拉伸*/
.box{
    -webkit-box-align: stretch;
    -moz-align-items: stretch;
    -webkit-align-items: stretch;
    align-items: stretch;
    box-align: stretch;
}
```
## 子元素伸缩
``` bash
.item{
    -webkit-box-flex:1.0;
    -moz-flex-grow:1;
    -webkit-flex-grow:1;
    flex-grow:1;
}
```
``` bash
.item{
    -webkit-box-flex:1.0;
    -moz-flex-shrink:1;
    -webkit-flex-shrink:1;
    flex-shrink:1;
}
```
## 子元素的显示次序
``` bash
.item{
    -webkit-box-ordinal-group:1;
    -moz-order:1;
    -webkit-order:1;
    order:1;
}
```
## 换行
``` bash
/*总*/
.box{
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```
``` bash
/*不换行*/
.box{
    flex-wrap: nowrap;
}
```
``` bash
/*以相反的顺序换行*/
.box{
    flex-wrap: wrap ;
}
```
