---
title: 手撕websocket
date: 2019-7-03 12:56:15
tags: basic
categories: Ajax
---

# WebSocket

初次接触 WebSocket 的人，都会问同样的问题：我们已经有了 HTTP 协议，为什么还需要另一个协议？它能带来什么好处？

答案很简单，因为 HTTP 协议有一个缺陷：通信只能由客户端发起。

WebSocket 协议在2008年诞生，2011年成为国际标准。所有浏览器都已经支持了。

它的最大特点就是，服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。

其他特点包括：

（1）建立在 TCP 协议之上，服务器端的实现比较容易。

（2）与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

（3）数据格式比较轻量，性能开销小，通信高效。

（4）可以发送文本，也可以发送二进制数据。

（5）没有同源限制，客户端可以与任意服务器通信。

（6）协议标识符是ws（如果加密，则为wss），服务器网址就是 URL

```javascript
      var ws = new WebSocket('wss://echo.websocket.org');

      ws.onopen = function (evt) {
          console.log('Connection open ...');
          ws.send('Hello WebSockets!');
      };

      ws.onmessage = function (evt) {
          console.log('Received Message: ', evt.data);
          ws.close();
      };

      ws.onclose = function (evt) {
          console.log('Connection closed.');
      };

      // url（必选），options（可选）
      fetch('/some/url/', {
          method: 'get',
      }).then(function (response) {

      }).catch(function (err) {
        // 出错了，等价于 then 的第二个参数，但这样更好用更直观
      });
```


参考文章：
[WebSocket 教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)