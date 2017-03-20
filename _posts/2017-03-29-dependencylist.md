---
layout: post
title:  "java maven 用到的dependency及功能记录"
date:   2017-03-20 15:06:18 +0800
categories: java
tags: java dependency
author: Over air
mathjax: true
---
* content
{:toc}

#### maven项目学习java，记录用到的maven

服务器跨域支持
```
<dependency>
    <groupId>com.thetransactioncompany</groupId>
    <artifactId>cors-filter</artifactId>
    <version>2.5</version>
</dependency>
```
文件上传读取
```
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.2</version>
</dependency>
```
图片滤镜处理
```
<dependency>
    <groupId>com.jhlabs</groupId>
    <artifactId>filters</artifactId>
    <version>2.0.235-1</version>
</dependency>
```
图片处理
```
<dependency>
    <groupId>org.im4java</groupId>
    <artifactId>im4java</artifactId>
    <version>1.4.0</version>
</dependency>
```
常用工具类
```
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
    <version>3.5</version>
</dependency>
```
RestFul接口返回json数据支持
```
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-core</artifactId>
    <version>2.2.3</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.2.3</version>
</dependency>
```
