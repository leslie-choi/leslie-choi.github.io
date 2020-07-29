---
title: 手撕jsonp
date: 2019-7-02 10:22:15
tags: basic
categories: Ajax
---

# 实现jsonp通信

JSONP(JSON with Padding) 是一种跨域请求方式。主要原理是利用了script 标签可以跨域请求的特性，由其 src 属性发送请求到服务器，服务器返回 JavaScript 代码，浏览器接受响应，然后就直接执行了，这和通过 script 标签引用外部文件的原理是一样的。

JSONP由两部分组成：**回调函数和数据** ，回调函数一般是在浏览器控制，作为参数发往服务器端（当然，你也可以固定回调函数的名字，但客户端和服务器端的名称一定要一致）。当服务器响应时，服务器端就会把该函数和数据拼成字符串返回。 
JSONP的请求过程：

请求阶段：浏览器创建一个 script 标签，并给其src 赋值(类似 http://example.com/api/?callback=jsonpCallback）。
发送请求：当给script的src赋值时，浏览器就会发起一个请求。
数据响应：服务端将要返回的数据作为参数和函数名称拼接在一起(格式类似”jsonpCallback({name: 'abc'})”)返回。当浏览器接收到了响应数据，由于发起请求的是 script，所以相当于直接调用 jsonpCallback 方法，并且传入了一个参数。

最简单版本：

```javascript
    let script = document.createElement("script")
    script.src = 'http://github.com&callback=callback'
    document.body.appendChild(script)
    function callback(ress){
        console.log(res)
    }
```



```javascript
//对请求data进行格式化处理
function formateData(data) {
    let arr = [];
    for (let key in data) {
        //避免有&,=,?字符，对这些字符进行序列化
        arr.push(encodeURIComponent(key) + '=' + data[key])
    }
    return arr.join('&');
}
 
//跨域jsonp请求
function jsonp(params) {
    //先对params进行处理，防止为空
    params = params || {};
    params.data = params.data || {};
    //后台传递数据时调用的函数名
    var callbackName = params.jsonp;
    // 拿到dom元素head，先不进行操作
    var head = document.querySelector('head');
    //创建script元素，先不进行操作
    var script = document.createElement('script');
    //传递给后台的data数据中，需要包含回调参数callback。
    //callback的值是 一个回调函数的函数名，后台通过该回调函数名调用传递数据，这个参数名的key由双方约定，默认为callback
    params.data['callback'] = callbackName;
    //对data数据进行格式化
    var data = formateData(params.data);
    //设置script请求的url跟数据
    script.src = `${params.url}?${data}`;
    //全局函数 由script请求后台，被调用的函数，只有后台成功响应才会调用该函数
    window[callbackName] = function (jsonData) {
        //请求移除scipt标签
        head.removeChild(script);
        clearTimeout(script.timer);
        window[callbackName] = null;
        params.success && params.success(jsonData)
    }
    //请求超时的处理函数
    if (params.time) {
        script.timer = setTimeout(() => {
            //请求超时对window下的[callbackName]函数进行清除，由于有可能下次callbackName发生改变了
            window[callbackName] = null;
            //移除script元素，无论请求成不成功
            head.removeChild(script)
            //这里不需要清除定时器了，clearTimeout(script.timer); 因为定时器调用之后就被清除了
 
            //调用失败回调
            params.error && params.error({
                message: '超时'
            })
        }, time);
    }
    //往head元素插入script元素，这个时候，script就插入文档中了，请求并加载src
    head.appendChild(script);
 
    //无论是请求超时，还是请求成功，都要移除script元素，script元素只有在第一次插入页面文档的时候，才会请求src
    //无论请求失败还是成功，都还是要移除window[callbackName]避免增加没用的全局方法，因为每次请求的callbackName可能是不同的
    //之前有个无聊的问题：为啥jsonp只能是get请求呢？看了实现过程，知道其实是因为script的加载就是get方式的~
}
```

```javascript
    <script>
//这个是jsonp的请求demo
        jsonp({
            url: 'http://127.0.0.1:3000/jsonp',
            jsonp: 'callback',
            data: {
                name: 'jgchen',
                stuNo: 2016130201,
                method: 'jsonp'
            },
            success(res) {
                console.log('jsonp success:',res);
            },
            error(err) {
                console.log(err);
            }
        })
    </script>
```