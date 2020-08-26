---
title: CSS常见知识点
date: 2019-4-22 10:59:25
tags: basic
categories: CSS
---

# css 命名规范（BEM）和优化

BlockElementModifier 其实是块（block）、元素（element）、修饰符（modifier）。

这三个部分使用 __ 与 -- 连接。

.块 __ 元素 -- 修饰符 {}

* block 代表了更高级别的抽象或组件。
* block__element 代表 block 的后代，用于形成一个完整的 block 的整体。
* block--modifier 代表 block 的不同状态或不同版本。

使用 BEM 命名规范的优点

1. CSS引擎查找样式表，对每条规则都按从右到左的顺序去匹配

```css
#ul-id li {}
```

这段代码看起来很快，实际上很慢。

通常我们会认为浏览器是这样工作的：找到唯一 ID 元素 ul-id —> 把样式应用到 li 元素上。

事实上: 从右到左进行匹配，遍历页面上每个 li 元素并确定其父元素。**所以尽量不要使 css 超过三层。**

eg： 相比于 # markdown-content-h3，显然使用 #markdown .content h3 时，浏览器生成渲染树（render-tree）所要花费的时间更多。后者需要先找到 DOM 中所有 h3 的元素，再过滤掉祖先元素不是 .content 的，最后再过滤祖先元素不是 #markdown 的元素。


2. 语义化

```css
/* .块 __ 元素 -- 修饰符 {} */
.person{ } /*人*/
.person__hand{ } /*人的手*/
.person--female{ } /*女人*/
.person--female__hand{ } /*女人的手*/
.person__hand--left{ } /*人的左手*/
```

3. 在 scss 中使用

**使用 @at-root 内联选择器模式，编译出来的 CSS 无任何嵌套（这是关键）**

```javascript
.person {
  @at-root #{&}__hand {
    color: red;
    @at-root #{&}--left {
     color: yellow;
    }
  }
  @at-root #{&}--female {
    color: blue;
    @at-root #{&}__hand {
      color: green;
    }
  }
}

/*生成的css*/

.person__hand {
   color: red;
}
.person__hand--left {
   color: yellow; 
}
.person--female{
  color: blue;
}
.person--female__hand {
  color: green;
}
```

4. 减少使用昂贵的属性

在浏览器绘制屏幕时，所有需要浏览器进行操作或计算的属性相对而言都需要花费更大的代价。

当页面发生重绘时，它们会降低浏览器的渲染性能。所以在编写CSS时，我们应该尽量减少使用昂贵属性，如box-shadow/border-radius/filter/透明度/:nth-child等。

当然，并不是让大家不要使用这些属性，因为这些应该都是我们经常使用的属性。之所以提这一点，是让大家对此有一个了解。

当有两种方案可以选择的时候，可以优先选择没有昂贵属性或昂贵属性更少的方案，如果每次都这样的选择，网站的性能会在不知不觉中得到一定的提升。

5. 减少回流和重绘


# Flex布局常见属性

## 1、flex-direction

flex-direction属性决定主轴的方向（即项目的排列方向）。 .box { display:flex; flex-direction: row | row-reverse | column | column-reverse; } row（默认值）：主轴为水平方向，起点在左端。 row-reverse：主轴为水平方向，起点在右端。 column：主轴为垂直方向，起点在上沿,自上而下。 column-reverse：主轴为垂直方向，起点在下沿,自下而上。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex1.PNG)

## 2、flex-wrap

//默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义 //如果一条轴线排不下，应该如何换行。 .box{ display:flex; flex-wrap: nowrap | wrap | wrap-reverse; } nowrap（默认）：不换行,宽度自动压缩。 wrap：换行，第一行在上方。 wrap-reverse：换行，第一行在下方。 ![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex2.PNG) ![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex3.PNG) ![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex4.PNG)

## 3、flex-flow

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式 ，默认值为row nowrap。 .box { flex-flow: <flex-direction> || <flex-wrap>; } .box{ flex-flow:row||nowrap; }

## 4、justify-content

justify-content属性定义了项目在主轴上(即横向)的对齐方式。

flex-start（默认值）：左对齐 flex-end：右对齐 center： 居中 space-between：两端对齐，组件之间的间隔都相等。 space-around：距边界两侧的间隔相等，元素之间的间隔比项目与边框的间隔大一倍。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex5.PNG)

## 5、align-items

align-items属性定义项目在交叉轴上(即纵向,垂直)如何对齐。 .box { align-items: flex-start | flex-end | center | baseline | stretch; } flex-start：交叉轴的起点(顶部)对齐。 flex-end：交叉轴的终点(底部)对齐。 center：交叉轴的中点(中间)对齐。 baseline: 项目的第一行文字的基线(即根据内容对齐,不再根据容器)对齐。 stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。 ![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex6.PNG)

## 6、align-content

align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。 .box { align-content: flex-start | flex-end | center | space-between | space-around | stretch; } flex-start：与交叉轴的起点对齐。 flex-end：与交叉轴的终点对齐。 center：与交叉轴的中点对齐。

space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。 space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。 stretch（默认值）：轴线占满整个交叉轴

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/flex7.PNG)

# link与@import的区别

区别1：link是XHTML标签，除了加载CSS外，还可以定义RSS等其他事务；@import属于CSS范畴，只能加载CSS。

区别2：link引用CSS时，在页面载入时同时加载；@import需要页面网页完全载入以后加载。

区别3：link是XHTML标签，无兼容问题；@import是在CSS2.1提出的，低版本的浏览器不支持。

区别4：link支持使用Javascript控制DOM去改变样式；而@import不支持。 　　import的写法比较多：推荐使用 @import url(index.css);

# src和href的区别

href标识超文本引用，用在link和a等元素上，href是引用和页面关联，是在当前元素和引用资源之间建立联系

src表示引用资源，表示替换当前元素，用在img，script，iframe上，src是页面内容不可缺少的一部分

src是source的缩写，是指向外部资源的位置，指向的内部会迁入到文档中当前标签所在的位置；在请求src资源时会将其指向的资源下载并应用到当前文档中，例如js脚本，img图片和frame等元素。

`<script src="js.js"></script>`当浏览器解析到这一句的时候会暂停其他资源的下载和处理，直至将该资源加载，编译，执行完毕，图片和框架等元素也是如此，类似于该元素所指向的资源嵌套如当前标签内，这也是为什么要把js饭再底部而不是头部。

`<link href="common.css" rel="stylesheet"/>`当浏览器解析到这一句的时候会识别该文档为css文件，会下载并且不会停止对当前文档的处理，这也是为什么建议使用link方式来加载css而不是使用@import。

# position属性

## 1、position:absolute;

绝对定位，**脱离文档流**的布局。相对于最近的已经定位的父元素，起始位置为最近的父元素(postion不为static)，否则为Body文档本身。

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>绝对定位</title>
<style>                
  body{background:green;}
  .parent{ width: 500px;height: 500px;background: #ccc;}
  .son{ width: 300px;height: 300px;background: #aaa;}
  span{position: absolute; right: 30px; background: #888;}
</style>
</head>
<body>
<div class="parent">
  <div class="son">
    <span>什么？</span>
  </div>
</div>
</body>
</html>
```

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/absolute2.png)

## 2、position:relative

**相对定位元素的定位是相对它自己的正常位置的定位**
所以关键在于如何确定其正常的位置。不会脱离文档流

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/relative.jpg)

在relative中，RLTB属性后的值如果是负数，则与其相同方向移动
如果值是正数，则与其反方向移动

## 3、position:fixed

fixed定位是指元素的位置相对于**浏览器窗口是固定位置**，即使窗口是滚动的它也不会滚动，且fixed定位使元素的位置与**文档流无关**，因此不占据空间，且它会和其他元素发生重叠。
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/fixed.png)
所以这个属性的应用就类似于网站的小广告一样！！！
但是这个属性在IE7和IE8不支持，需要另外声明。

## 4、position：static

默认的属性，只要有了这个属性，元素就会出现在**正常的文档流**，不会受到top、bottom、left和right的影响。
```html
<div class="wrap">
  <div class="content"></div>
</div>

<!--对应的css样式-->
.wrap{width: 300px;height: 300px; background: red;}
.content{position: static; top:100px; width: 100px;height: 100px; background: blue;}
```

## 5、position：sticky

粘性定位。
基于用户的滚动位置来定位。
粘性定位的元素是依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换。
它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。
元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

```javascript
<style>
  div.sticky {
    position: -webkit-sticky;
    position: sticky;
    top: 0;
    padding: 5px;
    background-color: #cae8ca;
    border: 2px solid #4CAF50;
  }
</style>

<div class="sticky">我是粘性定位!</div>

<div style="padding-bottom:2000px">
  <p>滚动我</p>
  <p>来回滚动我</p>
  <p>滚动我</p>
  <p>来回滚动我</p>
  <p>滚动我</p>
  <p>来回滚动我</p>
</div>
```

## 6、position：inherit

继承父类的position属性

## 7、position：initial      

将属性设置为初始值

**脱离文档流只有浮动属性，绝对定位以及固定定位**

# CSS清除浮动的几种方式

清除浮动的原因是由于高度的坍塌，本来父元素的高度是默认由子元素撑开的，但是由于子元素设置了浮动，则子元素完全脱离了文档流，导致子元素无法撑开父元素的高度，所以高度塌陷。

## 1、父级div定义伪元素：after和zoom

```html
<style type="text/css"> 
   .div1{background:#000080;border:1px solid red;}
   .div2{background:#800080;border:1px solid red;height:100px;margin-top:10px}
   
   .left{float:left;width:20%;height:200px;background:#DDD}
   .right{float:right;width:30%;height:80px;background:#DDD}
   
   /*清除浮动代码*/
   .clearfloat:after{
      display:block;
      clear:both;
      content:"";
      visibility:hidden;
      height:0;
   }
   .clearfloat{zoom:1}
</style> 


<div class="clearfloat"> 
	<div class="left">Left</div> 
	<div class="right">Right</div> 
</div>
<div class="div2">
   div2
</div>
```

IE8以上和非IE浏览器才支持:after，原理和方法2有点类似，==zoom(IE转有属性)==可解决ie6,ie7浮动问题。这个方法相对而言更加推荐使用。

## 2.在结尾处添加空div标签clear:both

```html
<style type="text/css"> 
   .div1{background:#000080;border:1px solid red}
   .div2{background:#800080;border:1px solid red;height:100px;margin-top:10px}
   .left{float:left;width:20%;height:200px;background:#DDD}
   .right{float:right;width:30%;height:80px;background:#DDD}
   
   /*清除浮动代码*/
   .clearfloat{clear:both}
</style> 
    <div class="div1"> 
    <div class="left">Left</div> 
    <div class="right">Right</div>
    <div class="clearfloat"></div>
</div>
<div class="div2">
   div2
</div>
```

不推荐使用，如果页面的浮动布局多，会新增了空的div，但此方法是以前主要使用的一种清除浮动方法，不过代码量少，对浏览器的兼容性好。

## 3、为父级元素增加高度

简单容易理解，但是需要给出精确的父级高度，适用于高度固定的布局。

## 4.父级div定义overflow:hidden

必须定义width或zoom:1，同时不能定义height，使用overflow:hidden时，浏览器会自动检查浮动区域的高度 简单，代码少，浏览器支持好，但是不能和position配合使用，因为超出的尺寸的会被隐藏。

# em和rem和vw/vh单位

这两个都是css单位，并且都是相对单位，先有的em，后有的rem。

## em

em可以让我们的页面更灵活，更健壮，比起到处写死的px值，em似乎更有张力，**改动父元素的字体大小，子元素会等比例变化**。但是使用em进行弹性布局，会使得单位之间的转换变得复杂，缺点还在于牵一发而动全身，一旦某个节点的字体大小发生变化，那么其后代元素都得重新计算。

```html
<div class="p1">
    <div class="s1">1</div>
    <div class="s2">1</div>
</div>
<div class="p2">
    <div class="s5">1</div>
    <div class="s6">1</div>
</div>

.p1 {font-size: 16px; line-height: 32px;}
.s1 {font-size: 2em;}
.s2 {font-size: 2em; line-height: 2em;}

.p2 {font-size: 16px; line-height: 2;}
.s5 {font-size: 2em;}
.s6 {font-size: 2em; line-height: 2em;}
```

line-height: 2自身字体大小的两倍

```html
p2：font-size: 16px; line-height: 32px
s5：font-size: 32px; line-height: 64px
s6：font-size: 32px; line-height: 64px
```

## rem

rem作用于非根元素时，相对于根元素字体大小；rem作用于根元素字体大小时，相对于其出初始字体大小。
实际开发可以设置网页的默认字体为50px，这样在换算成rem的时候直接将px的数值乘以0.2就可以了。
为了简便也可以使用webpack的px2rem-loader插件

```css
/* 作用于根元素，相对于原始大小（16px），所以html的font-size为32px*/
html{font-size:2rem}
/* 作用于非根元素，相对于根元素字体大小，所以为64px */
p{font-size:2rem}
```

（PS：**弹性布局可以算作响应式布局的一种，但响应式布局不是弹性布局**，弹性布局强调等比缩放，100%还原；响应式布局强调不同屏幕要有不同的显示，比如媒体查询）

缺点：如果要使用rem实现响应式的布局，则只要根据视图容器的大小，动态改变font-size就可以了，所以就必须通过js来动态控制根元素font-size的大小了，也就是说css样式和js有一定的耦合，而且必须把改变font-size的代码放在css样式之前，所以就有了另一种单位，vw和vh。

## vh和vw

视口：在桌面端，视口指的是在桌面端，指的是浏览器的可视区域；而在移动端，它涉及3个视口：Layout Viewport（布局视口），Visual Viewport（视觉视口），Ideal Viewport（理想视口）。

视口单位中的“视口”，桌面端指的是浏览器的可视区域；移动端指的就是Viewport中的Layout Viewport。

根据CSS3规范，视口单位主要包括以下4个：

1. vw：1vw等于视口宽度的1%。

2. vh：1vh等于视口高度的1%。

3. vmin：选取vw和vh中最小的那个。

4. vmax：选取vw和vh中最大的那个。

 vh and vw：相对于视口的高度和宽度，而不是父元素的（CSS百分比是相对于包含它的最近的父元素的高度和宽度）。1vh 等于1/100的视口高度，1vw 等于1/100的视口宽度。

比如：浏览器高度950px，宽度为1920px, 1 vh = 950px/100 = 9.5 px，1vw = 1920px/100 =19.2 px。

vmax相对于视口的宽度或高度中较大的那个。其中最大的那个被均分为100单位的vmax。

vmin相对于视口的宽度或高度中较小的那个。其中最小的那个被均分为100单位的vmin。

如果要兼容opera浏览器的话，不建议使用。

# flex实现圣杯布局和双飞翼布局

为什么要使用flex布局呢。因为比起绝对定位然后再去设置边距什么的，flex绝对容易理解然后代码量还少，DOM渲染时间更快，所以只讲flex，绝对不是因为懒！
圣杯布局：中间的div需要加上flex: 1，才可以将中间div的宽度填满。实现自适应宽度。
双飞翼布局和圣杯布局几乎一样(其实就是一回事)，区别在于处理center中被遮挡的部分。双飞翼是在center中再放一个div用来显示内容，为其设置margin。

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>圣杯布局</title>
	<style>
  * {
      margin: 0;
      padding: 0;
  }
	.container{
		display: flex;;
		justify-content: space-between;
	}
	.box{
		float: left;
		min-height: 200px;
	}
	.left{
		background: red;
		width: 200px;
	}
	.center{
		background: yellow;
		flex: 1;
	}
	.right{
		background: pink;
		width: 200px;
	}
	</style>
</head>

<body>

<div class="container">
	<div class="box left"></div>
	<div class="box center">233</div>
	<div class="box right"></div>
</div>

</body>
</html>
```

# CSS盒子模型

## 1、基本概念

CSS盒子模型主要有两种，一种是标准盒子模型，另外一种是IE盒子模型。接下来用图片来展示两种盒子模型。

标准盒子模型：
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/standardModel.jpg)

IE盒子模型：
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/IEmodel.jpg)


标准盒子模型的 width = content + padding + border + margin,高度同理。
IE盒子模型的 width = content + margin。
所以两个模型的主要区别在于IE模型的content其实包含了content和padding及border。

在现在的浏览器中，默认使用的是标准模型，二者的主要区别在于宽度和高度的计算方式不同。打开控制台就可以看到了。在这里，其实盒子模型的选取，看个人习惯吧，并没有绝对的好坏之分。
[markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/consoleBox.png)

## 2、切换两种盒子模型

有人开发可能习惯标准模型，有人可能习惯IE模型。那我们要怎么来切换这两种盒子模型呢？其实也是很简单，通过CSS3的box-sizing来设置就可以了。
box-sizing: content-box 是W3C盒子模型
box-sizing: border-box 是IE盒子模型

## 3、获取盒子宽度和高度

那么问题来了，如何通过JS来获取盒子模型的宽高呢。
a、使用dom.style.width/height

```javascript
		var oBox=document.getElementById("box");
		console.log(oBox.style.height);
		console.log(oBox.style.background);
		// console.log(oBox.currentStyle.width)
		// 上面的方法只是适用于IE
		console.log(window.getComputedStyle(oBox).width);
		console.log(oBox.getBoundingClientRect());
//dom这里就省略不写了。这个方法在这里只能获得内联样式，也就是如果你是外链的CSS样式文件，控制台是无法获得并打印出来的
```
b、dom.currentStyle.width/height
这个方法相比上一个方法的区别，就是获取的浏览器即时运行的结果，就是渲染以后的结果，相对准确，并且不局限于内联样式，但是这个方法只有IE能支持。

c、window.getComputedStyle(dom).width
道理其实和第二个是一样的，但是相对第二个该方法兼容了谷歌浏览器和火狐浏览器，所以兼容性更好一点。

d、dom.getBoundingClientRect().width/height
该方法通常用于计算一个元素的绝对位置，也就是根据视窗计算的。该方法会返回8个值上下左右宽高和xy

# BFC

首先什么是BFC，说实话这个概念之前都没有怎么接触过，所以赶紧找时间恶补了一下。。BFC全称是Block Formatting Context，即块格式化上下文。它是CSS2.1规范定义的，关于CSS渲染定位的一个概念。要明白深入了解BFC的话，需要先了解视觉格式化模型(visual formatting model)和CSS的盒子（BOX）概念。

## 视觉格式化模型

视觉格式化模型(visual formatting model)是用来处理文档并将它显示在视觉媒体上的机制，它也是CSS中的一个概念。
视觉格式化模型定义了盒（Box）的生成，盒主要包括了块盒、行内盒、匿名盒（没有名字不能被选择器选中的盒）以及一些实验性的盒（未来可能添加到规范中）。盒的类型由display属性决定。

### BOX

1. 块级盒：即display属性为block、list-item、table的元素，在视觉上呈现为块，竖直排列，独占一行。（支持宽高width、height）
2. 行内盒：即display的计算值为inline，inline-block或inline-table。
3. 匿名盒：匿名盒没有名字，不能利用选择器来选择它们，所以它们的所有属性都为inherit或初始默认值；

### 三个定位方案

* 常规流
* 浮动
* 定位

## BFC的形成条件

* 根元素
* float的值不能为none
* overflow的值不能为visible
* display的值为table-cell, table-caption, inline-block,flex,inline-flex中的任何一个
* position的值为absolute和fixed

## BFC的约束规则

1. 内部的Box会在垂直方向上一个接一个的放置
2. 垂直方向的距离有margin决定(属于同一个BFC的两个相邻Box的margin会发生重叠，与方向无关)
3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此
**4. BFC的区域不会与float的元素区域重叠**
**5. 计算BFC的高度时，浮动子元素也参与计算**
**6. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面元素，反之亦然**

## BFC的作用

### 不和浮动的元素相重叠
```html
	<section>
		<style>
		    .Box {
		        width: 300px;
		        position: relative;
		    }
		    .aside {
		        width: 100px;
		        height: 150px;
		        float: left;
		        background: #f66;
		    }

		    .main {
		        height: 200px;
		        background: #fcc;
		    }
		</style>

		<div class="Box">
			<div class="aside"></div>
			<div class="main"></div>
		</div>
	</section>
```
分析：
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/BFC1.png)
由于aside向左浮动具有BFC，但是main并不具有BFC，所以发生了重叠。在这里，根据上面的第四条规则，可以使main具备BFC，便不会发生重叠。
给main元素的overflow属性加上heiiden值便可满足条件。
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/BFC2.png)

### 清除元素内部的浮动

```html
<section>
		<style>
		    .par {
		        border: 5px solid #fcc;
		        width: 300px;
		    }

		    .child {
		        border: 5px solid #f66;
		        width:100px;
		        height: 100px;
		        float: left;
		    }
		</style>
	<div class="par">
	    <div class="child"></div>
	    <div class="child"></div>
	</div>
	</section>
```

分析：
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/BFC3.png)
根据BFC布局规则第六条：计算BFC的高度时，浮动元素也参与计算，为达到清除内部浮动，我们可以触发par生成BFC，那么par在计算高度时，par内部的浮动元素child也会参与计算。
所以在这里给par加上一个overflow的hidden属性值，触发par生成BFC就可以了。
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/BFC4.png)

### 防止垂直 margin 重叠

```html
	<section>
		<style>
		    p {
		        color: #f55;
		        background: #fcc;
		        width: 200px;
		        line-height: 100px;
		        text-align:center;
		        margin: 100px;
		    }
		</style>
		    <p>Haha</p>
		    <p>Hehe</p>
	</section>
```

分析：
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/BFC5.png)
很明显，在这里两个标签的margin发生了重叠，应该是200px。
根据BFC布局规则第二条：Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠，并且取值较大的那一个。
为了能够正常显示，那我们可以在p外面包裹一层容器，并触发该容器生成一个BFC。那么两个P便不属于同一个BFC，就不会发生margin重叠了。

```html
<style>
    .wrap {
        overflow: hidden;
    }
    p {
        color: #f55;
        background: #fcc;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 100px;
    }
</style>
<body>
    <p>Haha</p>
    <div class="wrap">
        <p>Hehe</p>
    </div>
</body>
```
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/BFC5.png)
按照BFC的定义，只有同属于一个BFC时，两个元素才有可能发生垂直Margin的重叠，这个包括相邻元素，嵌套元素，只要他们之间没有阻挡(例如边框，非空内容，padding等)就会发生margin重叠。
因此要解决margin重叠问题，只要让它们不在同一个BFC就行了，但是对于两个相邻元素来说，意义不大，没有必要给它们加个外壳，但是对于嵌套元素来说就很有必要了，只要把父元素设为BFC就可以了。这样子元素的margin就不会和父元素的margin发生重叠了。

# css选择器以及优先级

优先级从上往下递减。

1. !important
	在属性后面写上这条样式，会覆盖掉页面上任何位置定义的元素的样式。

2. 行内样式（1000），在style属性里面写的样式。

3. id选择器（100）	#box

4. class选择器和伪类选择器（10）.class{}

5. 标签选择器（1）	p ul li

6. 通配符选择器（0）*{}

7. 浏览器的自定义属性和继承

# 伪类和伪元素

**伪类包含两种：状态伪类和结构性伪类。** 

## 状态伪类是基于元素当前状态进行选择的。在与用户的交互过程中元素的状态是动态变化的，因此该元素会根据其状态呈现不同的样式。当元素处于某状态时会呈现该样式，而进入另一状态后，该样式也会失去。常见的状态伪类主要包括：

:link 应用于未被访问过的链接

:hover 应用于鼠标悬停到的元素

:active 应用于被激活的元素

:visited 应用于被访问过的链接，与:link互斥

:focus 应用于拥有键盘输入焦点的元素（比如输入框）

p:nth-child(2){} 规定属于其父元素的第二个子元素的每个 p 的背景色
PS:odd 和 even 是可用于匹配下标是奇数或偶数的子元素的关键词（第一个子元素的下标是 1）。

## 结构性伪类是css3新增选择器，利用dom树进行元素过滤，通过文档结构的互相关系来匹配元素，能够减少class和id属性的定义，使文档结构更简洁。常见的包括：

:first-child 选择某个元素的第一个子元素；

:last-child 选择某个元素的最后一个子元素；

:first-of-type 选择一个上级元素下的第一个同类子元素；

:last-of-type 选择一个上级元素的最后一个同类子元素；

等等。。。

**伪元素的本质是在不增加dom结构的基础上添加的一个元素，在用法上跟真正的dom无本质区别。普通元素能实现的效果，伪元素都可以。有些用伪元素效果更好，代码更精简。** 

一般使用单冒号(:)用于 CSS3 伪类，双冒号(::)用于 CSS3 伪元素

最常见的应用场景就是清除浮动了

```css
.clear:after {
    content: '';
    display: block;
    clear: both;
	  visibility: hidden;
	  height: 0;
}
```

## css3新增的其他伪类

:after 在元素之后添加内容，也可以用来清除浮动

:before 在元素之后添加内容

:enabled 已经启用的表单元素

:disabled 已经禁用的表单元素

:checked 单选框或复选框被选中

# css有哪些属性是可以继承的？

1. 字体系列

* font-family：字体系列
* font-weight：字体的粗细
* font-size：字体的大小
* font-style：字体的风格

2. 文本系列属性

* text-indent：文本缩进
* text-align：文本水平对齐
* line-height：行高
* word-spacing：单词之间的间距
* letter-spacing：中文或者字母之间的间距
* text-transform：控制文本大小写（就是uppercase、lowercase、capitalize这三个）
* color：文本颜色

3. 元素可见性

* visibility：控制元素显示隐藏

4. list-style：列表风格，包括list-style-type、list-style-image等

5. 光标属性：cursor：光标显示为何种形态

# CSS中哪些是不可继承的？

1. display属性

2. 文本属性

* vertical-align：垂直文本对齐
* text-decoration：规定添加到文本的装饰
* text-shadow：文本阴影效果
* white-space：空白符的处理
* unicode-bidi：设置文本的方向

3. 盒子模型属性

width、height、margin 、margin-top、margin-right、margin-bottom、margin-left、border、border-style、border-top-style、border-right-style、border-bottom-style、border-left-style、border-width、border-top-width、border-right-right、border-bottom-width、border-left-width、border-color、border-top-color、border-right-color、border-bottom-color、border-left-color、border-top、border-right、border-bottom、border-left、padding、padding-top、padding-right、padding-bottom、padding-left

4. 背景属性：

background、background-color、background-image、background-repeat、background-position、background-attachment

5. 定位属性：

float、clear、position、top、right、bottom、left、min-width、min-height、max-width、max-height、overflow、clip、z-index
等等。。。。

# 块级元素和行内元素

1. 块级元素，独占一行，会自动填满父元素，并且可以设置margin和padding以及宽度和高度(前后有换行符)

p div hn form table ul li ol dl

2. 行内元素，不会独占一行，并且宽度和高度会失效，垂直方向的padding和margin会失效(前后无换行符)

span a b input img i textarea select 

3. 行内块级元素，能设置宽度和高度，padding和margin水平垂直方向都有效(前后没有换行符)

# 居中为什么要使用transform（为什么不使用marginLeft/Top）

可以简单的这样理解，浏览器渲染的图层一般包含两大类：普通图层(默认复合图层)以及复合图层

可以通过硬件加速的方式，声明一个新的复合图层，它会单独分配资源 （当然也会脱离普通文档流，这样一来，不管这个复合图层中怎么变化，也不会影响默认复合层里的回流重绘）

transform是通过创建一个RenderLayers合成层，它所引起的paint也只是发生在单独的GraphicsLayer中，**并不会引起整个页面的回流重绘。**

层创立的条件如下：

* video元素的层
* canvas元素的层
* flash
* 对opacity和transform应用了CSS动画的层
* transform:translateZ(0)
* will-change: transform(对于接下来要做transform操作浏览器会为其新建一个图层)
* position: fixed

除此之外，在硬件加速体系结构，合成由GPU负责。在chrome浏览器多进程模型中，有一个专门的进程来负责传递Render进程的命令，即GPU进程。

Render进程和GPU进程是通过共享内存传递的。Render进程可以快速 的将命令发给命令缓冲区，并且返回到CPU密集的render活动中，留给GPU进程去处理这些命令。我们可以充分利用多内核机器上的GPU进程和CPU进程。这也是为什么GPU硬件加速会加速渲染，使transform渲染速度更快的又一原因。但是会占用一定的内存。

对元素进行动画的一些要点:
* 尽量使用keyframes和transform进行动画，这样浏览器会自身分配每帧的长度，并作出优化
* 如果非要使用js来进行动画，使用requestAnimationFrame
* 使用2d transform而不是改变top/left的值，这样会有更短的repaint时间和更圆滑的动画效果
* 移动端的动画效果可能会比pc端的差，因此一定要注意性能优化，尽量减少动画元素的DOM复杂性，待动画结束后异步执行DOM操作。

在实际开发的过程中，也应该对gif 动态图片新建立一个图层，但是图层太多，会导致页面耗费大量时间绘制图层，从而影响页面性能。所以必须平衡使用图层进行优化前后的方案，可以使用chrome 的performance 工具进行前后性能的对比。

# 层叠等级和层叠上下文

## 层叠上下文(stacking context)

HTML中一个三维的概念。在CSS2.1规范中，每个盒模型的位置是三维的，分别是平面画布上的X轴，Y轴以及表示层叠的Z轴。

一般情况下，元素在页面上沿X轴Y轴平铺，我们察觉不到它们在Z轴上的层叠关系。而一旦元素发生堆叠，这时就能发现某个元素可能覆盖了另一个元素或者被另一个元素覆盖。

如果一个元素含有层叠上下文，(也就是说它是层叠上下文元素)，我们可以理解为这个元素在Z轴上就“高人一等”，最终表现就是它离屏幕观察者更近。

## 层叠等级

层叠等级(stacking level，叫“层叠级别”/“层叠水平”也行)

* 在同一个层叠上下文中，它描述定义的是该层叠上下文中的层叠上下文元素在Z轴上的上下顺序。

* 在其他普通元素中，它描述定义的是这些普通元素在Z轴上的上下顺序。

## 结论

1. 普通元素的层叠等级优先由其所在的层叠上下文决定。

2. 层叠等级的比较只有在当前层叠上下文元素中才有意义。不同层叠上下文中比较层叠等级是没有意义的。

关键问题，如何产生层叠上下文？

其实，层叠上下文也基本上是有一些特定的CSS属性创建的，一般有3种方法

* HTML中的根元素本身就具有层叠上下文，称为“根层叠上下文”。
* 普通元素设置position属性为非static值并设置z-index属性为具体数值，产生层叠上下文。
* CSS3中的新属性也可以产生层叠上下文。(opacity属性值不是1;元素的transform属性值不是none)

举个例子：

```html
<style>
  div {
    width: 100px;
    height: 100px;
    position: relative;
  }
  .box1 {
    z-index: 2;
  }
  .box2 {
    z-index: 1;
  }
  p {
    position: absolute;
    font-size: 20px;
    width: 100px;
    height: 100px;
  }
  .a {
    background-color: blue;
    z-index: 100;
  }
  .b {
    background-color: green;
    top: 20px;
    left: 20px;
    z-index: 200;
  }
  .c {
    background-color: red;
    top: -20px;
    left: 40px;
    z-index: 9999;
  }
</style>
 
<body>
  <div class="box1">
    <p class="a">a</p>
    <p class="b">b</p>
  </div>
 
  <div class="box2">
    <p class="c">c</p>
  </div>
</body>
```
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/z-index.png)

虽然p.c元素的z-index值为9999，远大于p.a和p.b的z-index值，但是由于p.a、p.b的父元素div.box1产生的层叠上下文的z-index的值为2，p.c的父元素div.box2所产生的层叠上下
文的z-index值为1，所以p.c永远在p.a和p.b下面。
同时，如果我们只更改p.a和p.b的z-index值，由于这两个元素都在父元素div.box1产生的层叠上下文中，所以，谁的z-index值大，谁在上面。

**z-index 层级(仅能在定位元素上奏效，就是该元素的定位值为relative、absolute、或者fixed)。**

## 层叠顺序

在不考虑CSS3的情况下，当元素发生层叠时，层叠顺讯遵循下面图·中的规则

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/z-indexRules.png)


# display:none 和 visibility:hidden 以及 opacity:0 的区别

1. 空间占据

display:none隐藏后不占据额外空间，它会产生回流和重绘，而visibility:hidden和opacity:0元素虽然隐藏了，但它们仍然占据着空间，它们俩只会引起页面重绘。

2. 子元素继承

* display:none不会被子元素继承，但是父元素都不在了，子元素自然也就不会显示了

* visibility:hidden 会被子元素继承，可以通过设置子元素visibility:visible 使子元素显示出来

* opacity: 0 也会被子元素继承，但是不能通过设置子元素opacity: 0使其重新显示

3. 事件绑定

* display:none 的元素都已经不再页面存在了，因此肯定也无法触发它上面绑定的事件

* visibility:hidden 元素上绑定的事件也无法触发

* opacity: 0元素上面绑定的事件是可以触发的

4. 过渡动画

* transition对于display肯定是无效的

* transition对于visibility也是无效的

* transition对于opacity是有效

# display的值

inherit(继承父元素的display的值)		initial(初始值)

block	inline	none	flex	inline-block	inline-flex		inline-grid		inlie-table

grid	table	table-cell	table-caption	table-column	table-row	table-column-group	table-footer-group	table-header-group

等等。。

# 网页制作用到的图片格式

## 位图

1. 位图又叫做点阵图，是一个个很小的颜色小方块组合在一起的图片。一个小方块代表 1px（像素）。我们的手机屏幕和电脑屏幕也都是由很多个像素方块组成的

2. 常见的位图设计软件有 Photoshop(PS)\LR 等；常见的位图图片格式有 JPG、PNG、Bmp 等。所有位图图片都可以使用 PS 进行修改设计。在 PS 中把图片放大1600倍后，就可以看到一个个的像素点。类似马赛克的效果

## 矢量图

矢量图是由**一个个点链接在一起组成的，是根据几何特性来绘制的图像，和位图的分辨率是没有关系的**。因此图片放大后也不会失真，不会出现位图的马赛克的样子，也就是说可以无限放大图片。下图是用 AI 软件中放大6400倍的矢量图，依然很清晰

## 图片格式

1. png：支持 256 色调色板，静态图片，支持 alpha 透明度，无动画，适用图标、背景、按钮

2. GIF：多达256种的颜色，动态图片，适合简单动画

3. jpg：静态图片

4. WebP 格式，谷歌（google）新推出的影像技术，它可让网页图档有效进行压缩，同时又不影响图片格式兼容与实际清晰度，进而让整体网页下载速度加快 。图片压缩体积大约只有JPEG的2/3，并能节省大量的服务器带宽资源和数据空间。Facebook Ebay等知名网站已经开始测试并使用WebP格式。在质量相同的情况下，WebP格式图像的体积要比JPEG格式图像小40%，但是缺点是兼容性较差

5. apng 格式，说到动图，大家首先想到的肯定是 GIF。但 GIF 最大的缺点是，图像是基于颜色列表的（存储的数据是该点的颜色对应于颜色列表的索引值），最多只支持 8 位（256 色）。这使得使用 GIF 格式不可能得到高清的动画图片。**而 apng 是位图动画的拓展，可以实现 png 格式的动态图片效果**，有望代替 gif 成为下一代动态图的标准

* 支持 24 位真彩色图片
* 支持 8 位 Alpha 透明通道
* 向下兼容 PNG

## 其他图片知识

1. 雪碧图

优点：

* 减少 HTTP 请求数，极大地提高页面加载速度
* 增加图片信息的重复度，提高压缩比，减少图片大小

缺点：

* 图片合并麻烦
* 维护麻烦，修改一个图片可能需要重新布局整个图片、样式

2. base64 格式图片（在webpack 中可以使用 url-loader 进行图片打包）

优点：可以加密，减少 http 请求，适用于小图片（20kb左右），体积约为原图的4/3，一般比源文件大 30% 左右。

缺点：需要消耗cpu 进行编解码


# 参考文章：

[什么是BFC?](https://juejin.im/post/5a4dbe026fb9a0452207ebe6)
[学习 BFC (Block Formatting Context)](https://juejin.im/post/59b73d5bf265da064618731d)
[css BEM](https://www.jianshu.com/p/54b000099217)
[CSS性能优化的8个技巧](https://juejin.im/post/6844903649605320711)