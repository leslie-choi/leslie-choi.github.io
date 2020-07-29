---
title: CSS常见布局
date: 2019-5-21 20:29:25
tags: basic
categories: CSS

---
# 页面布局的几种方式

## 静态布局

1. 布局特点

　　不管浏览器尺寸具体是多少，网页布局始终按照最初写代码时的布局来显示。常规的pc的网站都是静态（定宽度）布局的，也就是设置了min-width，这样的话，如果小于这个宽度就会出现滚动条，如果大于这个宽度则内容居中外加背景，这种设计常见于pc端。

2. PC：居中布局，所有样式使用绝对宽度/高度(px)，设计一个Layout，在屏幕宽高有调整时，使用横向和竖向的滚动条来查阅被遮掩部分；

移动设备：另外建立移动网站，单独设计一个布局，使用不同的域名如wap.或m.根据浏览器用户代理，识别移动端，并重定向。

优点：这种布局方式对设计师和CSS编写者来说都是最简单的，亦没有兼容性问题。

缺点：显而易见，即不能根据用户的屏幕尺寸做出不同的表现。当前，大部分门户网站、大部分企业的PC宣传站点都采用了这种布局方式。固定像素尺寸的网页是匹配固定像素尺寸显示器的最简单办法。但这种方法不是一种完全兼容未来网页的制作方法，我们需要一些适应未知设备的方法。

## 流式布局

流式布局（Liquid）的特点（也叫"Fluid") 是页面元素的宽度按照屏幕分辨率进行适配调整，但整体布局不变。代表作栅栏系统（网格系统）。

网页中主要的划分区域的尺寸使用百分数（搭配min-*、max-*属性使用），例如，设置网页主体的宽度为80%，min-width为960px。图片也作类似处理（width:100%, max-width一般设定为图片本身的尺寸，防止被拉伸而失真）。

1. 布局特点

屏幕分辨率变化时，页面里元素的大小会变化而但布局不变。这就导致如果屏幕太大或者太小都会导致元素无法正常显示。

2. 设计方法

使用%百分比定义宽度，高度大都是用px来固定住，可以根据可视区域 (viewport) 和父元素的实时尺寸进行调整，尽可能的适应各种分辨率。往往配合 max-width/min-width 等属性控制尺寸流动范围以免过大或者过小影响阅读。

这种布局方式在Web前端开发的早期历史上，用来应对不同尺寸的PC屏幕（那时屏幕尺寸的差异不会太大），在当今的移动端开发也是常用布局方式，但缺点明显：主要的问题是如果屏幕尺度跨度太大，那么在相对其原始设计而言过小或过大的屏幕上不能正常显示。因为宽度使用%百分比定义，但是高度和文字大小等大都是用px来固定，所以在大屏幕的手机下显示效果会变成有些页面元素宽度被拉的很长，但是高度、文字大小还是和原来一样（即，这些东西无法变得“流式”），显示非常不协调

缺点：

* 计算困难
* 各个属性使用百分比，相对父元素的属性并不是唯一的。比如width和height相对于父元素的width和height。而margin和padding不管是垂直还是水平都相比父元素的宽度，border-radius则相对元素自身，导致了不一致性。

## 自适应布局

自适应布局的特点是分别为不同的屏幕分辨率定义布局，即创建多个静态布局，每个静态布局对应一个屏幕分辨率范围。改变屏幕分辨率可以切换不同的静态局部（页面元素位置发生改变），但在每个静态布局中，页面元素不随窗口大小的调整发生变化。可以把自适应布局看作是静态布局的一个系列。

1. 布局特点

屏幕分辨率变化时，页面里面元素的位置会变化而大小不会变化。

2. 设计方法

使用 @media 媒体查询给不同尺寸和介质的设备切换不同的样式。在优秀的响应范围设计下可以给适配范围内的设备最好的体验，在同一个设备下实际还是固定的布局。

## 响应式布局（Responsive Layout）

　　随着CSS3出现了媒体查询技术，又出现了响应式设计的概念。响应式设计的目标是确保一个页面在所有终端上（各种尺寸的PC、手机、手表、冰箱的Web浏览器等等）都能显示出令人满意的效果，对CSS编写者而言，在实现上不拘泥于具体手法，但通常是糅合了流式布局+弹性布局，再搭配媒体查询技术使用。——分别为不同的屏幕分辨率定义布局，同时，在每个布局中，应用流式布局的理念，即页面元素宽度随着窗口调整而自动适配。即：创建多个流体式布局，分别对应一个屏幕分辨率范围。可以把响应式布局看作是流式布局和自适应布局设计理念的融合。

　　响应式几乎已经成为优秀页面布局的标准。

1. 布局特点
　　每个屏幕分辨率下面会有一个布局样式，即元素位置和大小都会变。

2. 设计方法
　　媒体查询+流式布局。通常使用 @media 媒体查询 和网格系统 (Grid System) 配合相对布局单位进行布局，实际上就是综合响应式、流动等上述技术通过 CSS 给单一网页不同设备返回不同样式的技术统称。

优点：适应pc和移动端，如果足够耐心，效果完美。

缺点：
（1）媒体查询是有限的，也就是可以枚举出来的，只能适应主流的宽高。
（2）要匹配足够多的屏幕大小，工作量不小，设计也需要多个版本。

响应式页面在头部会加上这一段代码：
```html
<meta name="applicable-device" content="pc,mobile">
<meta http-equiv="Cache-Control" content="no-transform ">
```

## 弹性布局（rem/em布局）

1. rem/em区别：rem是相对于html元素的font-size大小而言的，而em是相对于其父元素。

2. 使用 em 或 rem 单位进行相对布局，相对%百分比更加灵活，同时可以支持浏览器的字体大小调整和缩放等的正常显示，因为em是相对父级元素的原因没有得到推广。

3. 这类布局的特点是，包裹文字的各元素的尺寸采用em/rem做单位，而页面的主要划分区域的尺寸仍使用百分数或px做单位（同「流式布局」或「静态/固定布局」）。早期浏览器不支持整个页面按比例缩放，仅支持网页内文字尺寸的放大，这种情况下。使用em/rem做单位，可以使包裹文字的元素随着文字的缩放而缩放。

4. 浏览器的默认字体高度一般为16px，即1em:16px，但是 1:16 的比例不方便计算，为了使单位em/rem更直观，CSS编写者常常将页面跟节点字体设为62.5%，比如选择用rem控制字体时，先需要设置根节点html的字体大小，因为浏览器默认字体大小16px*62.5%=10px。这样1rem便是10px，方便了计算。

5. 用em/rem定义尺寸的另一个好处是更能适应缩进/以字体单位padding或margin／浏览器设置字体尺寸等情况（因为em/rem相对于字体大小，会同步改变）。例如：p{ text-indent: 2em; }。

6. 使用rem单位的弹性布局在移动端也很受欢迎。

7. 其实在移动端使用所谓的弹性布局，是比较勉强的。移动端弹性布局流行起来的原因归根结底是rem单位对于（根据屏幕尺寸）调整页面的各元素的尺寸、文字大小时比较好用。其实，使用vw、vh等后起之秀的单位，可以实现完美的流式布局（高度和文字大小都可以变得“流式”），弹性布局就不再必要了。

## 结论：

1.如果只做pc端，那么静态布局（定宽度）是最好的选择；
2.如果做移动端，且设计对高度和元素间距要求不高，那么弹性布局（rem+js）是最好的选择，一份css+一份js调节font-size搞定；
3.如果pc，移动要兼容，而且要求很高那么响应式布局还是最好的选择，前提是设计根据不同的高宽做不同的设计，响应式根据媒体查询做不同的布局。


# 水平居中和垂直居中

## 水平居中

* 元素为⾏内元素，设置⽗元素 text-align:center 
* 如果元素宽度固定，可以设置左右 margin 为 auto
* 如果元素为绝对定位，设置⽗元素 position 为 relative ，元素设 left:0;right:0;margin:auto
* 使⽤ flex-box 布局，指定 justify-content 属性为center 
* display 设置为 tabel-ceil

## 垂直居中

* 将显示⽅式设置为表格， display:table-cell ,同时设置 vertial-align：middle 
* 使⽤ flex 布局，设置为 align-item：center 
* 绝对定位中设置 bottom:0,top:0 ,并设置 margin:auto 
* 绝对定位中固定⾼度时设置 top:50%，margin-top 值为⾼度⼀半的负值 
* ⽂本垂直居中设置 line-height 为 height 值

# 实现div的上下左右居中

## 1、绝对定位，左右距离50%，并且使margin减去宽高的一半

```css
	.div1{
		width: 200px;
		height: 200px;
		background: red;
		position: absolute;
		top: 50%;
		left: 50%;
		margin-left: -100px;
		margin-top: -100px;
	}
```

## 2、绝对定位，margin:auto的方法，并且设置TLBR都为0

```css
	.div2{
		width: 200px;
		height: 200px;
		background: red;
		position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
		margin: auto;
	}
```

## 3、绝对定位（上下左右各50%）和transform（CSS3）

```css
	.div3{
		width: 200px;
		height: 200px;
		background: red;
		position: absolute;
		top: 50%;
		left: 50%;
    transform: translate(-50%,-50%);
	}
```

## 4、flex布局居中（移动端使用无敌）

```css
	.parent{
		height: 500px;
		width: 500px;
		background: green;
		display: flex;
		justify-content: center;		//左右居中
		align-items: center;			//上下居中
	}
	.child{
		height: 200px;
		width: 100px;
		background: red;
	}
	<div class="parent">
		<div class="child"></div>
	</div>
```

注意内外层div都需要给他加上宽度，对应的display和justify-content、align-items是加在父级元素上。

## 5、在不固定div宽高的情况下，使用position:fixed和transform属性垂直居中

```css
.center {
  position: fixed;
  top: 50%;
  left: 50%;
  background-color: #000;
  width:auto;
  height: auto;
  transform: translate(-50%,-50%);
}
```

## 使用table布局

```css

#container     /**<img>的容器设置如下**/ 
{     
  display:table-cell;     
  text-align:center;     
  vertical-align:middle; 
}

```

# 实现文本垂直居中

## 实现单行文本垂直水平居中

```css

  <div class="box">hello world</div>
  .box{
    text-align:center;
    height:200px;
    line-height:200px;
    width:200px;
  }
```

## 实现多行文本垂直居中(登录框)

```css
  <style>
    #login{
        width: 300px;
        height: 300px;
        border: 1px black solid; 
        display: flex;
        flex-direction: column;        /*元素的排列方向为垂直*/
        justify-content: center;    /*水平居中对齐*/
        align-items: center;        /*垂直居中对齐*/
    }
  </style>

  <div id="login">
      <h1>登陆</h1>
      <input type="text"><br>
      <input type="password"><br>
      <button>确定</button>
   </div>

```

## 使用 display:table-cell

```css
  <section>
    <style>
  .box{
      width: 150px;
      height: 150px;
      background:blue;
      text-align: center;
      display: table-cell;
      vertical-align: middle;
  }
    </style>

      <div class="box">hello world</div>

  </section>
```

## 使用绝对定位和负边距

```css
<section>

<style>
        .box{
          width: 150px;
          height: 150px;
          background:blue;
          position: relative;
        }
        p{
          width: 50px;
          height: 50px;
          background:red;
          position: absolute;
          left:50%;
          top:50%;
          margin-left:-25px;
          margin-top: -25px;
          /*transform: translate(-25px,-40px);*/
          display: flex;
          align-items: center;
          justify-content: center;
        }
  </style>

<div class="box"><p>A</p></div>

</section>

```


# 圣杯布局和双飞翼布局

圣杯布局和双飞翼布局是前端工程师需要日常掌握的重要布局方式。两者的功能相同，都是为了实现一个两侧宽度固定，中间宽度自适应的三栏布局。

## 1、圣杯布局

a、首先dom结构左中右三栏由容器container包裹。并且必须按中左右这样排列下来
b、容器container的左边距是左栏的宽度，右边距是右栏的宽度，container也必须向左浮动
c、三栏向左浮动，设置高度
d、中间栏宽度100%
e、左边栏设置宽度，左边距设置-100%，位置相对定位relative，right属性值等于左边栏宽度（right:200px）
f、右边栏设置宽度，右边距设置-100%，

左右两栏的宽度已知。

DOM结构如下：
```html
    <div id="header"></div>
        <div id="container">
            <div id="center" class="column"></div>
            <div id="left" class="column"></div>
            <div id="right" class="column"></div>
        </div>
    <div id="footer"></div>
```

css如下：
```css
    body {
        min-width: 550px;
    }

    #container {
        padding-left: 200px; 
        padding-right: 150px;
    }

    #container .column {
        float: left;
        height: 200px;
    }

    #center {
        width: 100%;
        background: red;
    }

    #left {
        width: 200px; 
        margin-left: -100%;
        position: relative;
        right: 200px;
        background: yellow;
    }

    #right {
        width: 150px; 
        margin-right: -100%; 
        background: green;
    }

    #footer {
        clear: both;
        height:300px;
        background: black;
    }
```

## 2、双飞翼布局

双飞翼布局的DOM结构与圣杯布局的区别是用container仅包裹住center，另外将.column类从center移至container上。

a、容器container宽度width:100%；
b、column类设置左浮动和高度
c、中间栏设置高度，左边距和右边距分别是左边栏和右边栏的宽度
d、左边栏设置宽度，左边距-100%
f、右边栏设置宽度，左边距设置负的右边栏宽度（这里其实有点绕。。。）

DOM结构：
```html
<body>
    <div id="header"></div>
    <div id="container" class="column">
        <div id="center"></div>
    </div>
    <div id="left" class="column"></div>
    <div id="right" class="column"></div>
    <div id="footer"></div>
<body>
```

CSS：
```css
#container {
  width: 100%;
}

.column {
  float: left;
  height: 200px;
}
        
#center {
  margin-left: 200px;
  margin-right: 150px;
  background: red;
  height: 200px;
}
        
#left {
  width: 200px; 
  margin-left: -100%;
  background: yellow;
}
        
#right {
  width: 150px; 
  margin-left: -150px;
  background: blue;
}
        
#footer {
  clear: both;
}
```

# 常见的三栏布局（左右固定中间自适应）

首先在公共样式尚给所有的div加上一个最小宽度100px，有强迫症的可以再清除一下内外边距。
PS:<section> 标签定义了文档的某个区域。比如章节、头部、底部或者文档的其他区域。是HTML5的新标签
<article> 标签定义独立的内容。标签定义的内容本身必须是有意义的且必须是独立于文档的其余部分。也是H5新标签。潜在来源：论坛帖子、
博客文章、新闻故事、评论

## 1、使用浮动布局
结构不同于圣杯和双飞翼布局。前两个的布局是中间栏在前，然后左边栏右边栏。
使用浮动布局，则需要左右中。

a、dom结构左右中
b、左边栏设置宽度，左浮动
c、右边栏设置宽度，右浮动
d、中间栏不必设置宽度

```html
    <section class="layout float">
      <style media="screen">
      div{
        min-height:200px;
      }
      .layout.float .left{
        float:left;
        width:300px;
        background: red;
      }
      .layout.float .center{
        background: yellow;
      }
      .layout.float .right{
        float:right;
        width:300px;
        background: blue;
      }
      </style>
      <h1>三栏布局</h1>
      <article class="left-right-center">
        <div class="left"></div>
        <div class="right"></div>
        <div class="center">
          <h2>浮动解决方案</h2>
          1.这是三栏布局的浮动解决方案；
          2.这是三栏布局的浮动解决方案；
          3.这是三栏布局的浮动解决方案；
          4.这是三栏布局的浮动解决方案；
          5.这是三栏布局的浮动解决方案；
          6.这是三栏布局的浮动解决方案；
        </div>
      </article>
    </section>
```

## 2、使用绝对布局

dom结构左中右。

a、所有的div都设置绝对定位
b、左边栏设置宽度，left = 0；
c、中间栏不设置宽度，left等于左边栏宽度，right宽度等于右边栏宽度
d、右边栏设置宽度，right = 0

```html
    <section class="layout absolute">
      <style>
        div{
          min-height:200px;
        }
        .layout.absolute .left-center-right>div{
          position: absolute;
        }
        .layout.absolute .left{
          left:0;
          width: 300px;
          background: red;
        }
        .layout.absolute .center{
          left: 300px;
          right: 300px;
          background: yellow;
        }
        .layout.absolute .right{
          right:0;
          width: 300px;
          background: blue;
        }
      </style>
      <h1>三栏布局</h1>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h2>绝对定位解决方案</h2>
          1.这是三栏布局的浮动解决方案；
          2.这是三栏布局的浮动解决方案；
          3.这是三栏布局的浮动解决方案；
          4.这是三栏布局的浮动解决方案；
          5.这是三栏布局的浮动解决方案；
          6.这是三栏布局的浮动解决方案；
        </div>
        <div class="right"></div>
      </article>
    </section>
```

## 3、使用flex布局

dom结构顺序是左中右

a、需要一个额外的div包裹住这三栏，设置display属性为flex
b、设置左边栏的宽度
c、设置中间栏的flex数值为1
d、设置右边栏的宽度

```html
    <section class="layout flexbox">
      <style>
        .layout.flexbox{
          margin-top: 110px;
        }
        .layout.flexbox .left-center-right{
          display: flex;
        }
        .layout.flexbox .left{
          width: 300px;
          background: red;
        }
        .layout.flexbox .center{
          flex:1;
          background: yellow;
        }
        .layout.flexbox .right{
          width: 300px;
          background: blue;
        }
      </style>
      <h1>三栏布局</h1>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h2>flexbox解决方案</h2>
          1.这是三栏布局的浮动解决方案；
          2.这是三栏布局的浮动解决方案；
          3.这是三栏布局的浮动解决方案；
          4.这是三栏布局的浮动解决方案；
          5.这是三栏布局的浮动解决方案；
          6.这是三栏布局的浮动解决方案；
        </div>
        <div class="right"></div>
      </article>
    </section>
```
前面3个算是比较常用的了。接下来两个，大多数人也是接触过的。

## 4、表格布局

虽然说表格布局操作起来没有那么方便，但是相对其他的兼容性算好的。所以也要根据情况判断。

a、需要一个外层的div包裹住左中右三栏。设置宽度100%，高度自定义，display属性的值为table。
b、容器内的所有div的display属性设置为table-cell。
c、设置左右栏的宽度，中间栏自适应。

```html
    <section class="layout table">
      <style>
        .layout.table .left-center-right{
          width:100%;
          height: 100px;
          display: table;
        }
        .layout.table .left-center-right>div{
          display: table-cell;
        }
        .layout.table .left{
          width: 300px;
          background: red;
        }
        .layout.table .center{
          background: yellow;
        }
        .layout.table .right{
          width: 300px;
          background: blue;
        }
      </style>
      <h1>三栏布局</h1>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h2>表格布局解决方案</h2>
          1.这是三栏布局的浮动解决方案；
          2.这是三栏布局的浮动解决方案；
          3.这是三栏布局的浮动解决方案；
          4.这是三栏布局的浮动解决方案；
          5.这是三栏布局的浮动解决方案；
          6.这是三栏布局的浮动解决方案；
        </div>
        <div class="right"></div>
      </article>
    </section>
```

## 5、网格布局

dom结构依然是左中右，同样也需要一个外层的div包裹

a、外层包裹的div设置宽度100%，display的属性为grid。然后设置行和列
行：grid-template-rows:100px;
列：grid-template-columns:左边栏width auto 右边栏width;

b、完了

```html
    <section class="layout grid">
      <style>
        .layout.grid .left-center-right{
          width:100%;
          display: grid;
          grid-template-rows: 100px;
          grid-template-columns: 300px auto 300px;
        }
        .layout.grid .left-center-right>div{
        }
        .layout.grid .left{
          background: red;
        }
        .layout.grid .center{
          background: yellow;
        }
        .layout.grid .right{
          background: blue;
        }
      </style>
      <h1>三栏布局</h1>
      <article class="left-center-right">
        <div class="left"></div>
        <div class="center">
          <h2>网格布局解决方案</h2>
          1.这是三栏布局的浮动解决方案；
          2.这是三栏布局的浮动解决方案；
          3.这是三栏布局的浮动解决方案；
          4.这是三栏布局的浮动解决方案；
          5.这是三栏布局的浮动解决方案；
          6.这是三栏布局的浮动解决方案；
        </div>
        <div class="right"></div>
      </article>
    </section>
```

CSS的布局真的千变万化，不同的实现方式确实能体现出其掌握程度和水平。。。

# 实现文本的单行和多行溢出省略号

针对单行文本

```css
  {
    overflow: hidden;     /*溢出隐藏*/
    text-overflow:ellipsis;   /*文本溢出设置省略号*/
    white-space: nowrap;    /*设置不换行*/
  }
```
针对多行文本

```css
  {
    display: -webkit-box;
    -webkit-box-orient: vertical;   /*盒子右边设置垂直*/
    -webkit-line-clamp: 3;      /*设置行数*/
    overflow: hidden;       /*溢出隐藏*/
  }
```

# translate,transform,transition区别

1. translate:移动，transform的一个方法，通过 translate() 方法，元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标） 位置参数

2. transform:变形

* 旋转：rotate() 顺时针旋转给定的角度，允许负值 rotate(30deg)
* 扭曲：skew() 元素翻转给定的角度,根据给定的水平线（X 轴）和垂直线（Y 轴）参数：skew(30deg,20deg)
* 缩放：scale() 放大或缩小，根据给定的宽度（X 轴）和高度（Y 轴）参数： scale(2,4)
* 移动：translate() 平移，传进 x,y值，代表沿x轴和y轴平移的距离

3. transition: 允许CSS属性值在一定的时间区间内平滑的过渡，需要事件的触发，例如单击、获取焦点、失去焦点等。

* property:CSS的属性，例如：width height 为none时停止所有的运动，可以为transform
* duration:持续时间
* timing-function:ease等
* delay:延迟

# CSS3简单实现一个过渡动画

使用animation，动画定义了动作的每一帧有什么效果(也可以使用from、to来控制过渡的动画)

```css
div
{
	width:100px;
	height:100px;
	background:red;
	position:relative;
	animation:myfirst 5s;
}

@keyframes myfirst
/*{
    from {background: red;}
    to {background: yellow;}
}*/
{
	0%   {background:red; left:0px; top:0px;}
	25%  {background:yellow; left:200px; top:0px;}
	50%  {background:blue; left:200px; top:200px;}
	75%  {background:green; left:0px; top:200px;}
	100% {background:red; left:0px; top:0px;}
}

<div></div>
```

使用transition

```css
div{
  width:100px;
  height:100px;
  background:red;
  transition:margin-left 2s;
}

div:hover{
  margin-left:100px;
}

<div></div>
```

实现简单幻灯片效果

```css
div
{
	width:100px;
	height:100px;
	overflow:hidden;
	animation-name:myfirst;
	animation-duration: 5s;
	animation-iteration-count: infinite;	/*控制动画重复播放*/
  /*animation-delay 定义动画延迟播放的时间*/
  /*animationdirection 定义 动画的播放⽅向*/
  /*animation-iteration-count 定义播放次数*/
  /*animation-fill-mode 定义动画播放之后的状态、 */
  /*animation-play-state 定义播放状 态，如暂停运⾏*/
}

@keyframes myfirst
{
	0%   {background:url(https://b-gold-cdn.xitu.io/v3/static/img/book.75582b2.png)}
	50%  {background:url(https://b-gold-cdn.xitu.io/v3/static/img/juejin-extension-icon.4b79fb4.png)}
	100%  {background:url(https://b-gold-cdn.xitu.io/v3/static/img/juejin-partner.4dd2d8c.png)}
}
```


# CSS三角形

实现CSS三角形很简单，就是对div盒子加上边框，方向面对哪里，则对应的边框不用设置。

```css

<div class="sanjiaoxing"></div>
/*向上*/
.sanjiaoxing{
	width:0;
  height:0;
	border-right:50px solid transparent;
	border-left:50px solid transparent;
  border-top: 50px solid transparent;
	border-bottom:50px solid red;
}

/*向下*/
.sanjiaoxing{
	width:0;
  height:0;
	border-right:50px solid transparent;
	border-left:50px solid transparent;
  border-bottom:50px solid transparent;
	border-top:50px solid red;
}

```

# 常见两列布局

1. 第一列设置绝对定位，第二列设置margin-left。

```html
<section>
	<article>
		<style>
			#left{
			    position:absolute;
			    width:300px;
			    top:0px;
			    left:0px;
			    background:#F00;
			}
			#right{
			    background:#0FC;
			    margin-left:300px;
			}
		</style>

		<div id="left">左边定宽</div>
		<div id="right">右边自适应</div>
	</article>	
</section>
```

2. 第一列设置左浮动，第二列设置overflow:hidden

```html
<section>
	<style>
		#left {
		    float: left;
		    width: 300px;
		    background-color: blue;
		}
		#right {
		    overflow: hidden;
		    background-color: red;
		}
	</style>

<div id="left">左边自适应</div>
<div id="right">右边定宽</div>
</section>
```

3. 使用flex布局

```html
<section>
	<style>
		.container{
			display: flex;
		}
		#left{
			width: 300px;
			background: red;
			height: 100px;
		}
		#right{
			width: 300px;
			height:100px;
			background: yellow;
			flex: 1;
		}
	</style>

	<div class="container">
		<div id="left">左边自适应</div>
		<div id="right">右边定宽</div>
	</div>
</section>

```