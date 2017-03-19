---
layout: post
title:  "git学习 基础（二）"
date:   2017-03-19 10:26:18 +0800
categories: 版本控制
tags: git
author: Over air
mathjax: true
---
* content
{:toc}

#### 新建本地分支，拉去远程分支


###### 新建本地分支(branchname:dev)
```
git branch branchname
```
######  查看本地分支
```
git branch --list
  dev
* master
```
######  切换分支到dev
```
git checkout dev
Switched to branch 'dev'
//查看切换结果，*代表当前分支，其实git bash上会在后面用(dev)标志出来
git branch --list
* dev
  master
```
######  拉取远程分支dev
```
######  命令行标志
Max@Max-PC MINGW64 /giturl... (dev)//(dev)表示当前在本地dev分支上
git pull origin dev
From http://github...
 * branch            dev        -> FETCH_HEAD
Already up-to-date.
```
######  撤销本地commit更改文件
```
git checkout dir/filename
```
######  push代码到远程分支
```
git push origin dev
```
