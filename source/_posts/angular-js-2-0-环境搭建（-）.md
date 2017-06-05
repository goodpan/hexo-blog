---
title: angular.js 2.0 环境搭建（-）
date: 2016-10-20 09:30:30
tags: javascript
categories: Angular
---
## node环境
最好用node 6.x以上版本
## 安装Angular-CLI脚手架
``` bash
$ npm install -g angular-cli
```
## 创建项目目录
``` bash
$ npm new PROJECT_NAME
```
PROJECT_NAME 是项目名
## 安装项目依赖
``` bash
$ npm install
```
## 运行angular2项目
``` bash
$ cd PROJECT_NAME
$ ng serve
```
部署成功后不报错的情况下到浏览器 http://localhost:4200/，修改项目中文件后会自动部署
## angular2配置
配置默认的 HTTP 端口和一个 LiveReload server 用 --， 形如：
``` bash
ng serve --host 0.0.0.0 --port 4201 --live-reload-port 49153
```
