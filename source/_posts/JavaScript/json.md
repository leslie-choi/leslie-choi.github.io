---
title: JSON数据格式
date: 2019-01-20 13:22:43
categories: JavaScript
---

# JSON数据格式

emmmm。虽然很基础，但是作为js的一个子集（度娘也有文章说不是js的子集，但是这并不是重点），也是有必要将其单独提出来讲一下8。
JSON相比起XML格式的文档，优点就多了太多了。格式轻便，能够进行AJAX传输，可读性更高。。。

## 语法

JSON 数据的书写格式是：名称/值对。
名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值：

"name" : "吴彦祖"

JSON的取值可以是
1、数字（整数或浮点数）
2、字符串（在双引号中）
3、逻辑值（true 或 false）
4、数组（在中括号中）
5、对象（在大括号中）
6、null

一个简单的JSON对象：
```json
{
    "name" : "蔡徐坤",
    "age" : 18,
    "size" : "5cm",
    "height" : "160cm",
    "favorite" : [
        {"sport": "篮球","skills": "唱  跳 rap"}
    ]
}
```
## JSON处理数据的主要方法

### 使用JSON.stringify转换成字符串。

在向服务器发送数据时一般是字符串。
```javaScript
    var stu = {
                "name" : "蔡徐坤",
                "age" : 18,
                "size" : "5cm",
                "height" : "160cm",
                "favorite" : [
                    {"sport": "篮球","skills": "唱  跳 rap"}
                ]
            }
    var cxk = JSON.stringify(stu);
    console.log(cxk);
```

### 使用 JSON.parse() 方法将数据转换为 JavaScript 对象。
续上。

```javaScript
		var cxk = JSON.stringify(stu);
		console.log(JSON.parse(cxk));
```

PS:分别取出对象中的键和值
```javaScript
		var stu = {
				    "name" : "蔡徐坤",
				    "age" : 18,
				    "size" : "5cm",
				    "height" : "160cm",
				    "favorite" : [
				        {"sport": "篮球","skills": "唱  跳 rap"}
				    ]
				}
                
		for(let key in stu){
			console.log(key);
			console.log(stu[key]);
		}
```

# XML和JSON的区别

1. 数据体积⽅⾯
  JSON 相对 于XML 来讲，数据的体积⼩，传递的速度更快些。

2. 数据交互⽅⾯
  JSON 与 JavaScript 的交互更加⽅便，更容易解析处理，更好的数据交互

3. 数据描述⽅⾯
  JSON 对数据的描述性⽐ XML 较差

4. 传输速度⽅⾯
JSON 的速度要远远快于 XML
