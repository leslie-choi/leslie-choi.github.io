---
title: CSS预处理器
date: 2019-4-24 14:19:35
tags: basic
categories: CSS

---
# CSS预处理器

CSS预处理器主要有以下三个比较流行。sass、less和stylus。在vue项目中的使用方法，就是安装对应处理器的对应loader就可以了。
接下来讲一讲预处理器的几大特性。由于本人习惯于sass,至于less的语法其实和sass差不多，stylus那个复杂的写法第一次看到后直接劝退，所以下文主要是针对sass而言的。

## 1、节点可以嵌套，对父子级元素一目了然

```css

    //传统写法
    .parent {
        margin:10px;
    }
    .child {
        padding:10px;
    }
    a{
        font-size:16px;
    }
    //sass
    .parent{
        margin-10px;
            .child{
                padding: 10px;
                a{
                    font-size: 16px;
                }
            }
    }
```

## 2、可以声明变量

```css

//通常可以给网站的基础变量（基础字体、主题色调等）赋值一个变量，然后样式中调用这个变量就可以了。
//sass中变量的声明必须是一$号开头的
    $mainColor: #233233;
    $mainFont: 12px red;

    .header{
        background: $mainColor;
    }
```

## 3、使用Mixin，提高可重用性

通过@mixin加名称的方式就可以定义一个Mixins模块，在模块内你可以添加任何你想重复使用的样式。(比如清除浮动)

```css
@mixin button {  
    font-size: 1em;  
    padding: 0.5em 1.0em;  
    text-decoration: none;  
    color: #fff;  
}

//使用@include引入
.button-green {  
    @include button;  
    background-color: green;  
}

//解析之后的样式
.button-green {  
    font-size: 1em;  
    padding: 0.5em 1.0em;  
    text-decoration: none;  
    color: #fff;  
    background-color: green;  
}
//mixin另一个强大的地方是还可以接收参数！！无敌，定义一个全局的变量之后，就不必每次都去赋值了~~~

$background: red;
@mixin button($background) {  
    font-size: 1em;  
    padding: 0.5em 1.0em;  
    text-decoration: none;  
    color: #fff;  
    background: $background;  
}

```

## 4、 @extend继承选择器

```
　　.a1 {
　　　　color: blue;
　　}
　　.a2 {
　　　　@extend .a1;
　　　　font-size: 12px;
　　}
```

## 5、@function函数的功能
使用 @function+函数名称，每个函数都需要有返回值的内容

```
　　@function du($r) {
　　　　@return $r*2
　　}

　　.a8 {
　　　　border: solid #{du(2)}px red;
　　}
```

## 6、引用父元素&：在编译时，&将被替换成父选择符

```
　a {
　　　font-size: 20px;
　　　text-decoration: none;
　　　&:hover {
　　　　　text-decoration: underline;
　　　}
　}
//编译之后
　a {
　　　font-size: 20px;
　　　text-decoration: none;
　　}
　a:hover {
　　　text-decoration: underline;
　　}
```

以上算是sass中比较常见的用法吧。得益于sass，css的编写不再那样千篇一律。而scss则是sass的最新语法，完全兼容css，如果写习惯了css，可能不太习惯于那种严格的缩进式语法，相比其他几种，scss更适合我。。。。。