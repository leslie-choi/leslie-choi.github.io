---
title: 手撕Ajax
date: 2019-4-20 20:52:25
tags: basic
categories: Ajax
---

# 实现原生的AJAX通信

原生ajax请求

XMLHttpRequest 对象
XMLHttpRequest对象是ajax的基础,XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。目前所有浏览器都支持XMLHttpRequest。

onreadystatechange  每次状态改变所触发事件的事件。
responseText  从服务器接收到的响应体（不包括头部），或者如果还没有接收到数据的话，就是空字符串。
responseXML  对请求的响应，解析为 XML 并作为 Document 对象返回。
status  从服务器返回的数字代码，比如常见的404（未找到）和200（已就绪）
status Text   请求的 HTTP 的状态代码

readyState   HTTP 请求的状态.当一个 XMLHttpRequest 初次创建时，这个属性的值从 0 开始，直到接收到完整的 HTTP 响应，这个值增加到 4。
* 0 (未初始化) 对象已建立，但是尚未初始化（尚未调用open方法）
* 1 (初始化) 对象已建立，尚未调用send方法
* 2 (发送数据) send方法已调用，但是当前的状态及http头未知
* 3 (数据传送中) 已接收部分数据，因为响应及http头不全，这时通过responseBody和responseText获取部分数据会出现错误
* 4 (完成) 数据接收完毕,此时可以通过responseXml和responseText获取完整的回应数据


手撕原生Ajax完整版代码

```javascript
var ajax = function(param) {
    // 创建Ajax的核心对象，并且兼容IE低版本浏览器
   var xhr = XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");
   //对传入的值进行前期处理，包括type/url/data，将type值转换为大写
   var type = (param.type || 'get').toUpperCase();
   var url = param.url;
   if (!url) {
      return
   }
   var data = param.data,
       dataArr = [];
   for (var k in data) {
      dataArr.push(k + '=' + data[k]);
   }
   //为了避免取得的是缓存数据，为其添加一个唯一的id
   dataArr.push('_=' + Math.random());
   //get还是post。url处理，open()。send()
   if (type == 'GET') {
      url = url + '?' + dataArr.join('&');
      xhr.open(type, url);
      xhr.send();
   } else (type == 'POST') {
     xhr.open(type, url);
     //像表单一样post数据，使用setRequestHeader来添加头部
     xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
     xhr.send(dataArr.join('&'));
   }
   //接收请求
   xhr.onload = function() {
       //成功
   if (xhr.status == 200 || xhr.status == 304) {
      var res;
      if (param.success && param.success instanceof Function) {
          //获取字符串形式的响应数据
         res = xhr.responseText;
         if (typeof res === 'string') {
             //把字符串形式转换成JSON形式
            res = JSON.parse(res);
            param.success.call(xhr, res);
         }
      }
    }else{
		//失败
			if(param.error && param.error instanceOf Function){
				param.error.call(xhr,res);
			}
	    }
  };
};
```
方法的使用：
```javaScript
    <script>
        //这个是ajax的get跟post请求demo
        ajax({
            type: 'post',
            url: 'http://127.0.0.1:3000/ajax',
            data: {
                name: 'jgchen',
                stuNo: 2016130201,
                method: 'post'
            },
            success(res) {
                console.log('POST success:',res);
            },
            error(err) {
                console.log(err);
            }
        })
        ajax({
            type: 'GET',
            url: 'http://127.0.0.1:3000/ajax',
            data: {
                name: 'jgchen',
                stuNo: 2016130201,
                method: 'get'
            },
            success(res) {
                console.log('GET success:',res);
            },
            error(err) {
                console.log(err);
            }
        })
    </script>
```


get请求

```javascript
//步骤一:创建异步对象
    var ajax = new XMLHttpRequest();
//步骤二:设置请求的url参数,参数一是请求的类型,参数二是请求的url,可以带参数,动态的传递参数starName到服务端
    ajax.open('get','getStar.php?starName='+name);
//步骤三:发送请求
    ajax.send();
//步骤四:注册事件 onreadystatechange 状态改变就会调用
    ajax.onreadystatechange = function () {
        if (ajax.readyState==4 &&ajax.status==200) {
    //步骤五 如果能够进到这个判断 说明 数据 完美的回来了,并且请求的页面是存在的
　　　　console.log(ajax.responseText);//输入相应的内容
  　　    }
    }
```

post请求

```javascript
//创建异步对象  
    var xhr = new XMLHttpRequest();
//设置请求的类型及url，必须要先open才能继续下一步
	xhr.open('post', '02.post.php' );
//post请求一定要添加请求头才行不然会报错
	xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");

//发送请求
	xhr.send('name=fox&age=18');
	xhr.onreadystatechange = function () {
    // 这步为判断服务器是否正确响应
    if (xhr.readyState == 4 && xhr.status == 200) {
        console.log(xhr.responseText);
      } 
    };
```

参考文章：
[终于明白如何去写原生AJAX](https://juejin.im/post/5d007134e51d45590a445b23)
[聊聊Ajax那些事](https://segmentfault.com/a/1190000006669043) 