---
title: java EE三层架构开发模式
date: 2018-10-26 20:12:32
tags: basic
categories: java
typora-copy-images-to: vue\images
---

三层架构开发模式前面关于login和register功能已经有所涉及到一点，但是由于业务简单所以我的代码没有按照这个模式来，所以接下来的功能我都会按照这个模式来开发，因为这样可以使得我们的开发思路更加清晰。

![Markdown](http://i4.bvimg.com/645823/212f26a953a5f121.png)

如上图所示，web客户端就是我们平时接触到的网页程序，通过传递参数给服务器，然后服务器根据请求再与数据库连接，然后获取到数据库数据之后服务器再将其结果返回到前端页面。而在这个过程之中，服务器端又可以将其分成三层，即web层，service层还有dao层。其中，web层是用来接收客户端传输过来的数据，然后再将其封装到JavaBean中，传递给sevice层，其中使用到的框架有Struts2。而service层接收web层传递过来的数据，然后根据实际需求编写业务逻辑代码。最后一个是dao层，service层所需要用到的数据，则是通过dao层来执行对数据库的操作，然后再一层一层返回，最终在客户端显示结果。这样做的好处是可以使开发的思路更加清晰，各层分工明确。

至于说到MVC模式和三层架构之间的关系，可以说Mvc是web开发的一种开发模式，而三层架构是针对Java语言的。本身是没有联系，但是三层架构中，分别是web层，service层还有dao层，servlet+JavaBean+jsp刚刚好构成了web层，所以web层是一种mvc的模式。（wen层中，JavaBean是M层，jsp是V层，Servlet是C层）

这个也有可能是面试题目哦，所以最好记住也不要混淆，嘿嘿嘿嘿~  

![vue增删改](D:\leslie-choi.github.io\source\_posts\vue\images\vue增删改.png)