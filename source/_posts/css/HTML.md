---
title: HTML
date: 2019-7-23 15:19:35
tags: basic
categories: CSS
---

# HTML语义化

在我的理解为：根据内容，使用最合适的HTML标签。
场景：加入一个文章标题，要求这个标题的字体比正文的要大一些，还要加粗。能够实现这种效果的方法有很多，比如用CSS样式进行渲染。这样的效果看起来像是一个标题，但是他对浏览器来说，只是一个被渲染过的文本，无法知道他是一个标题。若要让浏览器知道他是一个标题，应该用hn标签来进行标记。
从这个例子可以总结出： 语义化的HTML文档，不关心内容的显示效果。 说的通俗一点： 标题脱了CSS这层外衣，它还是一个标题。

## 语义化标签的优势

* 代码结构清晰，方便阅读，有利于团队合作开发。
* 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以语义的方式来渲染网页。
* 有利于搜索引擎优化（SEO）。

## HTML5新增的语义化标签

* header：页眉通常包括网站标志、主导航、全站链接以及搜索框
* nav：标记导航，仅对文档中重要的链接群使用
* main：页面主要内容，一个页面只能使用一次。如果是web应用，则包围其主要功能
* article：定义外部的内容，其中的内容独立于文档的其余部分
* section：定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分
* footer：页脚，只有当父级是body时，才是整个页面的页脚

# meta标签的理解和总结

meta常用于定义页面的说明，关键字，最后修改日期，和其它的元数据。它不会显示在页面上，但是机器却可以识别。
这些元数据将服务于浏览器（如何布局或重载页面），搜索引擎和其它网络服务。

**组成：meta标签一共有两个属性，分别是http-equiv属性和name属性**

## name属性

name属性主要用于描述网页，比如网页的关键词，叙述等。与之对应的属性值为content，content中的内容是对name填入类型的具体描述，便于搜索引擎抓取。
meta标签中name属性语法格式是：

```html
<meta name="参数" content="具体的描述">
```

* **A. keywords(关键字)**
说明：用于告诉搜索引擎，你网页的关键字。

```html
<meta name="keywords" content="Lxxyx,博客，文科生，前端">
```

* **B. description(网站内容的描述)**
说明：用于告诉搜索引擎，你网站的主要内容。

```html
<meta name="description" content="文科生，热爱前端与编程。目前大二，这是我的前端博客">
```

（SEO优先级按照title、description、keywords的顺序递减）

* **C. viewport(移动端的窗口)**
说明：这个概念较为复杂，具体的会在下篇博文中讲述。
这个属性常用于设计移动端网页。在用bootstrap,AmazeUI等框架时候都有用过viewport。

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

* **C. viewport(移动端的窗口)**
说明：robots用来告诉爬虫哪些页面需要索引，哪些页面不需要索引。
content的参数有all,none,index,noindex,follow,nofollow。默认是all。

```html
<meta name="robots" content="none">
```

* **E. author(作者)**
说明：用于标注网页作者

```html
<meta name="author" content="leslie,leslie_choi@yeah.net">
```

* **G. copyright(版权)**
说明：用于标注版权信息

```html
<meta name="copyright" content="leslie"> //代表该网站为leslie个人版权所有。
```

## http-equiv属性

equiv的全称是"equivalent"，意思是相等，相当于。
所以在这里的作用就是相当于HTTP的作用，类似于定义http参数。
```html
<meta http-equiv="参数" content="具体的描述">
```
* **A. content-Type(设定网页字符集)(推荐使用HTML5的方式)**

说明：用于设定网页字符集，便于浏览器解析与渲染页面

```html
<meta http-equiv="content-Type" content="text/html;charset=utf-8">  //旧的HTML，不推荐

<meta charset="utf-8"> //HTML5设定网页字符集的方式，推荐使用UTF-8
```

* **B. X-UA-Compatible(浏览器采取何种版本渲染当前页面)**

说明：用于告知浏览器以何种版本来渲染页面。（一般都设置为最新模式，在各大框架中这个设置也很常见。）

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/> //指定IE和Chrome使用最新版本渲染当前页面
```

* **C.cache-control(指定请求和响应遵循的缓存机制)**

**用法一：指导浏览器如何缓存某个响应以及缓存多长时间。**

```html
<meta http-equiv="cache-control" content="no-cache">
```
共有以下几种用法：

1、no-cache: 先发送请求，与服务器确认该资源是否被更改，如果未被更改，则使用缓存。

2、no-store: 不允许缓存，每次都要去服务器上，下载完整的响应。（安全措施）

3、public : 缓存所有响应，但并非必须。因为max-age也可以做到相同效果

4、private : 只为单个用户缓存，因此不允许任何中继进行缓存。（比如说CDN就不允许缓存private的响应）

5、maxage : 表示当前请求开始，该响应在多久内能被缓存和重用，而不去服务器重新请求。例如：max-age=60表示响应可以再缓存和重用 60 秒。

**用法二：禁止百度自动转码。**

用于禁止当前页面在移动端浏览时，被百度自动转码。虽然百度的本意是好的，但是转码效果很多时候却不尽人意。所以可以在head中加入例子中的那句话，就可以避免百度自动转码了。

```html
<meta http-equiv="Cache-Control" content="no-siteapp" />
```

* **D. expires(网页到期时间)**

用于设定网页的到期时间，过期后网页必须到服务器上重新传输。

```html
<meta http-equiv="expires" content="Sunday 26 October 2016 01:00 GMT" />
```

* **refresh(自动刷新并指向某页面)**

网页将在设定的时间内，自动刷新并调向设定的网址。

```html
<meta http-equiv="refresh" content="2；URL=http://www.baidu.com"> //意思是2秒后跳转
```

* **Set-Cookie(cookie设定)** 

如果网页过期。那么这个网页存在本地的cookies也会被自动删除。

```html
<meta http-equiv="Set-Cookie" content="name, date"> //格式

<meta http-equiv="Set-Cookie" content="User=Lxxyx; path=/; expires=Sunday, 10-Jan-16 10:00:00 GMT"> //具体范例
```

## 设置页面不缓存的方法

```html

<meta http-equiv="pragma" content="no-cache"> 
<meta http-equiv="Cache-Control" content="no-cache, must-revalidate"> 
<meta http-equiv="expires" content="0">

```

# H5和CSS3的新特性

## html5有哪些新特性

1. 拖拽释放(Drag and drop) API 
2. 语义化更好的内容标签（header,nav,footer,aside,article,section）
3. 音频、视频API(audio,video)
4. 画布(Canvas) API
5. 地理(Geolocation) API
6. 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；sessionStorage 的数据在浏览器关闭后自动删除。
7. sessionStorage 的数据在浏览器关闭后自动删除
8. 表单控件，calendar、date、time、email、url、search  
9. 新的技术webworker, websocket, Geolocation

### html5 的离线存储以及工作原理

在⽤户没有与因特⽹连接时，可以正常访问站点或应⽤，在⽤户与因特⽹连接时，更新⽤ 户机器上的缓存⽂件

原理：HTML5 的离线存储是基于⼀个新建的 .appcache ⽂件的缓存机制(不是存储技 术)，通过这个⽂件上的解析清单离线存储资源，这些资源就会像 cookie ⼀样被存储了下 来。之后当⽹络在处于离线状态下时，浏览器会通过被离线存储的数据进⾏⻚⾯展示

使用方法：

1. ⻚⾯头部像下⾯⼀样加⼊⼀个 manifest 的属性

```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">
...
</html>
```

2. 在 cache.manifest ⽂件的编写离线存储的资源 

3. 在离线状态时，操作 window.applicationCache 进⾏需求实现

在线的情况下，浏览器发现 html 头部有 manifest 属性，它会请求 manifest ⽂件，如 果是第⼀次访问 app ，那么浏览器就会根据manifest⽂件的内容下载相应的资源并且进⾏ 离线存储。如果已经访问过 app 并且资源已经离线存储了，那么浏览器就会使⽤离线的资 源加载⻚⾯，然后浏览器会对⽐新的 manifest ⽂件与旧的 manifest ⽂件，如果⽂件没有发⽣改变，就不做任何操作，如果⽂件改变了，那么就会重新下载⽂件中的资源并进⾏离线存储。

离线的情况下，浏览器就直接使⽤离线存储的资源。

## CSS3 新增属性

1. 颜色：新增RGBA，HSLA模式
2. 文字阴影（text-shadow、）
3. 边框： 圆角（border-radius）边框阴影： box-shadow
4. 盒子模型：box-sizing
5. 背景：background-size 设置背景图片的尺寸background-origin 设置背景图片的原点。background-clip 设置背景图片的裁切区域，以”，”分隔可以设置多背景，用于自适应布局
6. 渐变：linear-gradient、radial-gradient
7. 过渡：transition，可实现动画
8. 自定义动画
9. 在CSS3中唯一引入的伪元素是 ：：selection.
10. 媒体查询，多栏布局
11. border-image
12. 2D转换：transform：translate(x，y) rotate(x，y) skew(x，y) scale(x，y)


## H5中已经废弃的标签

* br 换行，已经被 p 标签进行替换
* hr 画线
* font、big、center
* 在企业开发中，不到万不得已 不要轻易使用被废弃的标签
* strong ：定义重要性强调的文字
* ins 定义插入的文字
* em 定义强调的文字
* del定义被删除的文字
* frame 等等

# 参考文章：
[关于 HTML 中 meta 标签的理解和总结](https://juejin.im/entry/588074c62f301e00696b481d)