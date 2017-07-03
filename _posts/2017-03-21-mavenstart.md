---
layout: post
title:  "maven构建springmvc项目实践"
date:   2017-03-21 19:58:18 +0800
categories: java
tags: maven
author: Over air
mathjax: true
---
* content
{:toc}

#### maven空项目引入springMvc
需要的dependency
```xml
<!--版本号-->
<properties>
    <spring.version>4.1.1.RELEASE</spring.version>
</properties>

<!--添加的dependency-->
<dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webMvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>
  </dependencies>

```
添加完成，修改web.xml,配置severlet。
```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
    <servlet-name>mvc-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>mvc-dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

</web-app>

```
在WEB-INF文件夹下，添加一个名字为mvc-dispatcher-servlet.xml的配置文件 <br/>
注意：该文件名字的命名有要求。默认时，web.xml文件中指定的是什么，则配置文件的名字就是该名字-servlet.xml。当然也可以进行额外的配置改变这种默认的文件命名方式以及文件的位置
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--目录-->
    <context:component-scan base-package="com.springapp.mvc"/>

    <!-- 启动注解驱动的Spring MVC功能，注册请求url和注解POJO类方法的映射-->
    <mvc:annotation-driven />

    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```
在main文件下建java目录，并修改文件属性为源代码，如下图 <br/>
![修改属性](http://gitpages-1251551899.picgz.myqcloud.com/5-12-3.png)

OK，springMvc添加进去了。想继续添加monggodb的，后来折腾了大半天`Spring Data & MongoDB`,发现不好用啊 <br>
由于本来就是把mongo当附数据库来时用的，直接用`MongoClient`，存文档简单。比较简单，不再叙述。
