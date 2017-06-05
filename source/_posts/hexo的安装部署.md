
---
title: hexo 安装部署
date: 2016-10-18 10:44:19
tags: hexo
---

## 安装node

## 安装git

## 配置github
* 新建一个仓库
  注意仓库名字：比如我的Github账号是goodpan，那么我应该创建的Repository的名字是：goodpan.github.io。
## 安装hexo
``` bash
    npm install -g hexo
```
## 启动hexo
* 新建文件夹，如H:\Hexo
* 进入Hexo文件中，右键打开Git Bash
* 执行下面的命令：
``` bash
  hexo init Hexo
```
  随后会自动在目标文件夹建立网站所需要的文件
* 在Hexo文件中打开终端，运行npm install，安装依赖
* 执行：hexo server启动hexo服务,出现：Hexo is running athttp://localhost:4000/表明hexo server已经启动
* 在浏览器中打开 http://localhost:4000/
## hexo命令
* 在H:\hexo目录下打开一个git bash命令行窗口，执行命令：
``` bash
  hexo new "My New Post"
```
  刷新http://localhost:4000/，可以发现已生成了一篇新文章 "My New Post"。
* 执行下面的命令，将markdown文件生成静态网页：hexo generate，该命令执行完后，会在 D:\Hexo\public\ 目录下生成一系列html，css等文件。
## 部署到github
* 配置_config.yml文件
首先找到下面的内容:
  deploy:
  type:
  然后将它们修改为
  deploy:
  type:  git
  repository:  https://github.com/goodpan/goodpan.github.io.git
  branch: master
* 配置说明：
1. type在git 3.0后填写为git而非github
2. type、reoisitory、branch必须有空格，如果格式出错会报如下错误：
  FATAL bad indentation of a mapping entry at line 72, column 15:
* 依次执行以下命令部署：
``` bash
  hexo clean

  hexo generate

  hexo deploy
```
* 错误解决
1. ERROR Local hexo not found in ~/blog ERROR Try runing: ‘npm install hexo –save’

  执行命令：
``` bash
  npm install hexo --save
```
## 使用yeoman生成基础代码
1. 安装yeoman
``` bash
npm install -g yo
```
2. 安装生成器的库
``` bash
npm install -g generator-hexo-theme
```
3. 创建themes主题
进入themes目录下新建一个用主题命名的新文件夹，设置权限，执行以下命令生成主题模板：
``` bash
yo hexo-theme
```
按照提示进行相应配置
