---
layout: struts 2
title: Struts 2（一）
date: 2018-11-01 19:46:27
tags:
typora-root-url: img
---

# 										Struts 2（一）

Struts 2是一个基于MVC设计模式的web应用框架，本质上是相当于一个servlet。Struts 2是以WebWork为核心，采用拦截器的机制来处理用户的请求，使得业务逻辑控制器与ServletApi完全脱离，所以Struts2是一个基于MVC设计模式的WEB框架。常见的WEB层框架还有SpringMVC，Webwork。

![Markdown](http://i1.bvimg.com/645823/f91f9de15bc67453.png)

Struts2环境搭建

1、到官网下载Struts2的开发环境 ：http://struts.apache.org/

2、解压Struts2的开发包。然后搜索到struts-blank项目，这个项目是一个最基本的Struts项目，找到其中的lib目录，引入jar包。

​    ![Markdown](http://i1.bvimg.com/645823/a5b1968f0ac718be.png)

3、创建一个jsp页面 

![Markdown](http://i1.bvimg.com/645823/4c27715088bd1ae8.png)

4、编写对应的Action类，不同于传统的创建servlet，也不需要实现其他的接口

<img src='http://i2.bvimg.com/645823/f5ead895f5ce8203.png'>

5、然后对Action进行配置

在src目录下面创建名称为struts.xml的配置文件

![Markdown](http://i2.bvimg.com/645823/a38f3415d845492a.png)

6、然后再WEB-INF下面的web.xml配置前端控制器（核心过滤器）

![Markdown](http://i2.bvimg.com/645823/1b01e347a0c6167e.png)

7、改写Action中的方法的返回值

![Markdown](http://i2.bvimg.com/645823/28854e29b3af2aa4.png)

8、然后改写Struts.xml，配置页面的跳转

http://i2.bvimg.com/645823/66dc493b00ed988e.png

9、编写success.jsp

![Markdown](http://i2.bvimg.com/645823/99e244c47aa1e8a9.png)

![air](/air.png)

   ![lucky](/../Struts2/lucky.jpg)