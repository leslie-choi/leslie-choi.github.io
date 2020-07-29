---
title: my first blog
date: 2018-10-21 16:59:35
tags:
---

  nothing。。。



# 常见网络知识点

## 1、常见的http状态码
状态代码有三位数字组成，第一个数字定义了响应的类别，且有五种可能取值：
1xx：指示信息--表示请求已接收，继续处理
2xx：成功--表示请求已被成功接收、理解、接受
3xx：重定向--要完成请求必须进行更进一步的操作
4xx：客户端错误--请求有语法错误或请求无法实现
5xx：服务器端错误--服务器未能实现合法的请求

200 OK 一切正常，对GET和POST请求的应答文档跟在后面。

201 Created服务器已经创建了文档，Location头给出了它的URL。

202 Accepted 已经接受请求，但处理尚未完成。

304 Not Modified 客户端有缓冲的文档并发出了一个条件性的请求（一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档）。服务器告 诉客户，原来缓冲的文档还可以继续使用。

400 Bad Request 请求出现语法错误。

404 Not Found 无法找到指定位置的资源。这也是一个常用的应答。

405 Method Not Allowed 请求方法（GET、POST、HEAD、DELETE、PUT、TRACE等）对指定的资源不适用。（HTTP 1.1新）

500 Internal Server Error 服务器遇到了意料不到的情况，不能完成客户的请求。

502 Bad Gateway 服务器作为网关或者代理时，为了完成请求访问下一个服务器，但该服务器返回了非法的应答。

## 2、http协议

HTTP协议是**Hyper Text Transfer Protocol（超文本传输协议）**的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议，也就是说所有的www服务器都必须要遵循这个标准。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

HTTP协议工作于客户端-服务端架构为上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。Web服务器根据接收到的请求后，向客户端发送响应信息。
![http](images/http.jpg)

HTTP请求由三部分组成，分别是：请求行、请求头、请求正文

请求方法（所有方法全为大写）有多种，各个方法的解释如下：
GET     请求获取Request-URI所标识的资源
POST    在Request-URI所标识的资源后附加新的数据
HEAD    请求获取由Request-URI所标识的资源的响应消息报头
PUT     请求服务器存储一个资源，并用Request-URI作为其标识
DELETE  请求服务器删除Request-URI所标识的资源
TRACE   请求服务器回送收到的请求信息，主要用于测试或诊断
CONNECT 保留将来使用
OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求

HTTP缺点：
1、通信使用明文（ 不加密） ， 内容可能会被窃听
2、不验证通信方的身份， 因此有可能遭遇伪装
3、无法证明报文的完整性， 所以有可能已遭篡改

## 3、Https
Https协议其实是基于Http的，多了个s其实就是Secure，所以也就是**Hypertext Transfer Protocol Secure（超文本传输安全协议）**
相对于HTTP协议，具有以下优点：
1.  内容加密：采用混合加密技术，中间者无法直接查看明文内容
2.  验证身份：通过证书认证客户端访问的是自己的服务器
3.  保护数据完整性：防止传输的内容被中间人冒充或者篡改
![https](images/https.png)
### 1）、SSL和TLS

SSL(Secure Sockets Layer 安全套接层),及其继任者传输层安全（Transport Layer Security，TLS）是为网络通信提供安全及数据完整性的一种安全协议。TLS与SSL在传输层对网络连接进行加密

## 4、TCP协议

![TCP](images/TCP.png)
互联网由一整套协议构成
图片说明：顶层协议是应用层协议 => TCP协议 => IP协议 => 以太网协议

以太网协议：最底层的协议，可以解决子网内部的点对点通讯。

IP协议：IP协议，可以解决多个局域网之间互通，可以连接多个局域网，定义了一套自己的地址规则，实现了路由的功能，允许某个局域网的A主机，向另一个局域网的B主机发送消息。（PS：路由器就是基于IP协议。所以局域网之间要靠路由器连接）

题外话：路由原理就是路由器内部有一张路由表，规定A段IP地址走出口一，B段IP地址走出口二。。。根据这样实现了数据包的转发。

TCP协议：IP协议只是一个地址协议，并不能保证数据的完整性。如果缓存满了，新进来的数据包就会丢失，所以这个时候需要发现丢了哪一个包，以及如何重新发送这个包，所以这个时候需要使用到TCP协议。以太网数据包的负载是1500字节，TCP数据包的负载在1400字节左右。

## 5、URL和URI的区别
![URL](URL.png)
URL是Uniform Resource Locator，表示是一个地址。
URI是Uniform Resource Identifier,表示是一个资源。
URL是URI的子集，所有的URL都是URI，但不是每个URI都是URL，还有可能是URN。
在这里举一个栗子：比如这个世界上的电音之王是加拿大炮王XXX。那这个头衔电音之王加拿大炮王就是一个URI，那如果我们想要找到这个人XXX，光知道头衔肯定是不够的，所以我们还需要知道他的住址，而这个住址就是URL。

同理，每个人的身份证号码就是一个URI，而每个人的家庭住址也是一个URL。
总结：**URI强调的是给资源标记命名，URL强调的是给资源定位，**但是你会发现，URL显然比URI包含信息更多，我通过URL也可以知道张三是总经理，并且我还知道了他的地址，**所以大多数情况下大家觉得给一个网络资源分别命名和给出地址太麻烦，干脆就用地址既当地址用，又当标记名用**，所以，URL也充当了WWW万维网里面URI的角色，但是他比URI多了一层意义，我不光知道你叫什么，我还知道你在哪里。我们在浏览器输入的都是URL，因为我们输入的目的是为了找到某一个资源，当然你输入的是URI也是没错的，因为URL也是URI。


## 6、跨域问题

    跨域，是指浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对

JavaScript实施的安全限制。
同源策略限制了一下行为：

Cookie、LocalStorage 和 IndexDB 无法读取
DOM 和 JS 对象无法获取
Ajax请求发送不出去。

所谓的同源是指，域名、协议、端口均为相同
<http://www.nealyang.cn/index.html> 调用   <http://www.nealyang.cn/server.php>  非跨域

<http://www.nealyang.cn/index.html> 调用   <http://www.neal.cn/server.php>  跨域,主域不同

<http://abc.nealyang.cn/index.html> 调用   <http://def.neal.cn/server.php>  跨域,子域名不同

<http://www.nealyang.cn:8080/index.html> 调用   <http://www.nealyang.cn/server.php>  跨域,端口不同

<https://www.nealyang.cn/index.html> 调用   <http://www.nealyang.cn/server.php>  跨域,协议不同

localhost   调用 127.0.0.1 跨域
### 解决跨域问题
	1、jsonp解决跨域

jsonp跨域其实也是JavaScript设计模式中的一种代理模式。
在html页面中通过相应的标签从不同域名下加载静态资源文件是被浏览器允许的，所以我们
可以通过这个“犯罪漏洞”来进行跨域。一般，我们可以动态的创建script标签，再去请求一个带参
网址来实现跨域通信

```javascript
//原生的实现方式	
let  script  =  document.createElement('script');
script.src  =  'http://www.nealyang.cn/login?username=Nealyang&callback=callback';
document.body.appendChild(script);

function  callback(res)  {  
    console.log(res);
}

//Jquery实现方式
$.ajax({
    url:'http://www.nealyang.cn/login',
    type:'GET',
    dataType:'jsonp',//请求方式为jsonp
    jsonpCallback:'callback',
    data:{
        "username":"Nealyang"
    }
})
```

jsonp的最大缺点，就是只能实现get请求

另外使用script标签元素进行Ajax请求，这意味着允许Web页面
**可以执行远程服务器发送过来的任何JavaScript代码，因此对于不信任的服务器，不应该采用该技术**。
2、**跨域资源共享 CORS-----目前主流的跨域解决方案**
CORS是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。
IE8+：IE8/9需要使用XDomainRequest对象来支持CORS。
整个CORS通信过程，都是浏览器自动完成，不
需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。
浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的
请求，但用户不会有感觉。
因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就
可以跨源通信。

一种是简单请求，另一种是非简单请求。只要满足下面条件就是简单请求

请求方式为HEAD、POST 或者 GET
http头信息不超出一下字段：
Accept、Accept-Language 、 Content-Language、 Last-Event-ID、 Content-Type(限于三
个值：application/x-www-form-urlencoded、multipart/form-data、text/plain)

为什么要分为简单请求和非简单请求，因为浏览器对这两种请求方式的处理方式是不同的。

使用方法很简单，在服务端设置：

Header set Access-Control-Allow-Origin *
这种设置将接受所有域名的跨域请求，也可以指定具体网址，也可以对域名进行限定：
Header set Access-Control-Allow-Origin http://www.test.com

	然而，这种方式的局限性在于要求客户端支持，并且服务端进行相关设置。
在这两者满足的情况下，通过CORS进行跨域不仅支持所有类型的HTTP请求，而且开发者可以使用
普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。

	3）利用window.name进行跨域处理
	4）使用window.postMessage进行跨域

## 7、Cookie、session和localStorage、以及sessionStorage之间的区别

前景引入：http是一种无状态的协议，它的无状态可以用翻脸不认人（浏览器）来表示了；以至于服务器不会记得前一秒是哪个客户端向它发出了请求，这就会导致一种情况出现：小熊登录了淘宝；跳转一下页面，需要再登录；加入购物车，需要再登录；付款需要再登陆…影响用户体验。 
在服务端的解决办法是：用session去管理cookie。

当程序需要为某个客户端的请求创建一个session时，服务器首先检查这个客户端的请求里是否已包含了一个session标识（称为session id），如果已包含则说明以前已经为此客户端创建过session，服务器就按照session id把这个session检索出来使用（检索不到，会新建一个），如果客户端请求不包含session id，则为客户端创建一个session并且生成一个与此session相关联的session id，session id的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个session id将被在本次响应中返回给客户端保存。保存这个session id的方式可以采用cookie，这样在交互过程中浏览器可以自动的按照规则把这个标识发送给服务器。

### cookie

内容主要包括**名字、值、过期时间、路径和域**。路径与域一起构成cookie的作用范围。若不设置时间，则表示这个cookie的生命期为浏览器会话期间，关闭浏览器窗口，cookie就会消失。这种生命期为浏览器会话期的cookie被称为会话cookie。所以会话cookie是保存在内存中，如果设置了过期时间，那么cookie就会保存在磁盘里。

### session

在WEB开发中，服务器可以为每个用户浏览器创建一个会话对象（session对象），注意：一个浏览器独占一个session对象(默认情况下)。因此，在需要保存用户数据时，服务器程序可以把用户数据写到用户浏览器独占的session中，当用户使用浏览器访问其它程序时，其它程序可以从用户的session中取出该用户的数据，为用户服务。

### localStorage

在HTML5中，新加入了一个localStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题(cookie中每条cookie的存储空间为4k)，**localStorage中一般浏览器支持的是5M大小**，这个在不同的浏览器中localStorage会有所不同。

### sessionStorage

sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。（能够保证当前会话或者表单信息不丢失，比如刷新页面不丢失表单数据，填写多个表单可以返回上一步查看数据等）

## session和cookie的主要区别

*   Cookie是把用户的数据写给用户的浏览器。
*   Session技术把用户的数据写到用户独占的session中。
*   Session对象由服务器创建，开发人员可以调用request对象的getSession方法得到session对象。
*   cookie不是很安全，别人可以分析存放在本地的cookie并进行cookie欺骗，考虑到安全应当使用session
*   session会在一定时间内保存在服务器上，当访问增多，会比较占用你服务器的性能，考虑到减轻服务器性能方面，应当使用cookie。
*   单个cookie保存的数*据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
*   session中保存的是对象，cookie中保存的是字符串


## **sessionStorage、localStorage和cookie的区别** 
共同点：都是保存在浏览器端、且同源的 
区别： 
1. cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下 

2. 存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大 

3. 数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭之前有效；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭 

4. 作用域不同，**sessionStorage不在不同的浏览器窗口中共享**，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的