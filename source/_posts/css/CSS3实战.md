<!--
 * @Description: 
 * @Author: caixg
 * @Date: 2020-11-19 15:38:29
 * @LastEditors: caixg
 * @LastEditTime: 2020-11-20 10:42:02
-->
# 新增选择器

1. 属性选择器语法

* E[title] 选择匹配 E 的元素，并且该元素定义了 title 的属性。
* E[title="bar"] 选择匹配 E 的元素，并且该元素定义了 title 的属性，并且属性值 为 bar。
* E[title*="bar"] * 号代表匹配任意的字符。
* E[title^="bar"] ^ 号代表匹配起始符号。
* E[title$="bar"] $ 号代表匹配终止符号。

```html
  <!-- 属性自定义 my-test 属性 -->
  <a href="" my-test="bar">属性选择器</a>
  
  <style>
    [my-test='bar'] {
      color: red;
    }
  </style>
```

Ps：利用属性选择器的特性，可以匹配超链接对应的图标。不必为每个超链接加上类名。

```css
a[href^="http:"] { /* 匹配所有有效的超链接*/
  
}
a[href$="pdf"] { /* 匹配 pdf 文件*/
  
}
a[href$="ppt"] { /* 匹配 ppt 文件*/
  
}
/* 其他文件同理 */
```

2. 状态性伪类

* E:link 链接伪类选择器，选择匹配 E 的元素，并且匹配的元素被定义了超链接但是**没有被访问。**
* E:visited 链接伪类选择器，选择匹配 E 的元素，并且匹配的元素被定义了超链接但是**已经被访问。**
* E:active 用户操作伪类选择器，选择匹配 E 的元素，且匹配元素被激活。
* E:focus 用户操作伪类选择器，选择匹配 E 的元素，且匹配元素获取了焦点。
* E:hover 用户操作伪类选择器，选择匹配 E 的元素，且匹配元素正被鼠标经过。
* **E::first-line 伪元素选择器，匹配 E 元素内的第一行文本。**
* **E::first-letter 伪元素选择器，选择匹配 E 元素内的第一个字符。**

# 伪元素的使用

伪元素的本质是在不增加 DOM 结构的基础上添加的一个元素，在用法上跟真正的 DOM 无本质区别。
普通元素能实现的效果，伪元素都可以。有些用伪元素效果更好，精简 DOM 结构。
一般使用单冒号 (:) 用于 CSS3 伪类，双冒号 (::) 用于 CSS3 伪元素。

1. 实现分割线

```html
<style>
.divider::before,.divider::after{
  content: '';
  display: inline-block;
  border-top: 1px solid black;
  width: 200px;
  margin: 5px;
}
</style>

<div class="divider">分割线</div>
```

2. attr 函数定义伪元素内容

```html
<style>
.ku::before{
  /*position: absolute;*/
  font-size: 2rem;
  color: #333;
  content: attr(data-text); /* 将特定的属性值作为插入的文本成为一个伪元素*/
  white-space:nowrap;/*
  width: 50%;
  display: inline-block;
  overflow: hidden*/
}
</style>

<span data-text='库' class="ku">库</span>
```

3. 清除浮动

```css
.clear:after {
  content: '';
  display: block;
  clear: both;
  visibility: hidden;
  height: 0;
}
```