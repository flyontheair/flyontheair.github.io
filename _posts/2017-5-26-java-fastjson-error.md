---
layout: post
title:  "fastjson报错"
date:   2017-05-26 10:25:18 +0800
categories: java
tags: java
author: Max
mathjax: true
---

* content
{:toc}

#### fastjson.jsonexception:default constructor not found
在解析式会报错说构造函数没有，原来是我改写了实体类构造函数后，没有显式的声明无参构造函数
> 构造函数：当你一个也没写的时候,JVM就自己给一个无参默认的.当你写了时,JVM就不会给了。所以，如果父类没有无参数的构造方法的话，子类的构造方法中必须显式调用父类的带参数的构造方法。
```java
public class ProductEntity extends BaseEntity {

	public String productId;

	public int channelType;

	public long channelId;

	public String channelName;

	//显式声明
	public ProductEntity() {
	}


	public ProductEntity(String productId, int channelType, long channelId, String channelName) {
		super();
		this.productId = productId;
		this.channelType = channelType;
		this.channelId = channelId;
		this.channelName = channelName;
	}
}
```
