---
title: DOM事件类
date: 2019-06-14 23:56:24
tags: Interview
categories: JavaScript
---

# DOM事件的级别

(注意DOM1设计的时候，没有设计与事件相关的东西，所以在这里就没有谈及DOM1了)
1. DOM0  - >  element.onclik = function(){}
2. DOM2  - >  element.addEventListener('click',function(){},false)
3. DOM3  - >  element.addEventListener('keyup',function(){},false)
参数true则为事件捕获，参数false则为事件冒泡
DOM3和DOM2其实相差不多，只是DOM3的事件类型增加了很多，鼠标的移入移出和键盘的监听等等

# DOM事件模型

1. 事件捕获

```html
<body>

<div>
    <button>
        <p>点击捕获</p>
    </button>
</div>
<script>
    var oP=document.querySelector('p');
    var oB=document.querySelector('button');
    var oD=document.querySelector('div');
    var oBody=document.querySelector('body');

    oP.addEventListener('click',function(){
        console.log('p标签被点击')
    },true);

    oB.addEventListener('click',function(){
        console.log("button被点击")
    },true);

    oD.addEventListener('click',  function(){
        console.log('div被点击')
    },true);

    oBody.addEventListener('click',function(){
        console.log('body被点击')
    },true);

</script>

```
输出的结果是body ->  div -> button -> p。
addEventListener的第三个参数默认是false。
改为false即为冒泡了。

2. 事件冒泡

```javascript
    <div class="box1" onclick="box1()">
        <ul onclick="ul()">
            <li onclick="li()"></li>
        </ul>
    </div>

    <script>

        function box1(){
            console.log("box1被点击了")
        }
        function ul(){
            console.log("ul被点击了")
        }
        function li(){
            console.log("li被点击了")
        }
    </script>
```
点击li，则输出的结果为 li -> ul -> box1

说到事件代理，则有必要了解一下。事件代理也叫事件委托，利用冒泡阶段的运行机制实现的，就是把一个元素响应事件的函数委托到另一个元素，一般是把一组元素的事件委托到他的父元素上面，实际运用场景：可以在父元素层面阻止事件向子元素传播，也可以代替子元素执行某些操作。
优点：
* 减少内存消耗，提高效率
* 动态事件绑定（）

```javascript
//打印每个项的索引

<ul id="tab" >
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>
</ul>


let lists = document.getElementsByTagName('li')


//注意：如果不使用事件代理，则需要使用为每一个项注册点击事件，然后变量使用闭包保存
  for(let i = 0;i<lists.length;i++){
    lists[i].addEventListener('click',function(){
      return function(i){
        console.log(i)
      }(i)
    })
  }

//使用事件代理的机制
		let tab = document.getElementById('tab')
		tab.onclick = function(event){
			for(let i = 0;i< lists.length;i++){
				if(lists[i] == event.target){
					console.log(i)
					return i
				}
			}
		}
```

# DOM事件流

一个完整的事件流包括了3个阶段，事件捕获 - > 目标阶段 - > 冒泡阶段 

# 描述DOM事件捕获的具体流程

window -> document -> html -> body -> ... -> 目标元素
（冒泡则相反）

# Event对象的常见应用

1. event.preventDefault()       - > 阻止默认事件
2. event.stopPropagation()      - > 阻止冒泡
3. event.stopImmediatePropagation()     - > 事件的响应优先级。（场景：按钮绑定两个事件a和b，要求点击按钮的时候a事件触发的同时不会触发b，则可以在其中加入该代码）
4. event.currentTarget    - > 指的是当前绑定的事件
5. event.target           - > 事件代理情况下使用。（场景：有一个ul列表下面有很多的li标签，每个li都可以触发一个事件，那么可以将该事件委托在其父元素ul上，但是触发事件的时候，就要用到event.target判断是哪一个li触发了事件）

# 自定义事件（模拟事件）

```javascript
	var ev = document.getElementsByClassName("box1");

    var eve = new Event('cusstome'); //eve类似于click，传入的参数是事件名称
    ev.addEventListener('custome',function(){   //ev是获取到的DOM节点
        console.log("custome")
    });
    ev.dispatchEvent(eve);
```
还有一个CustomEvent，用法一样，不过CustomerEvent()可以多传一个对象参数。