---
title: JS常见API
date: 2019-02-23 18:06:21
tags: basic
categories: JavaScript
---

# 获取DOM节点

第一个返回选中id，其余则是返回一个数组。所以注意是elements,并且获取的时候要使用elements[0]的数组形式获取否则报错

* document.getElementById()
* document.getElementsByName()
* document.getElementsByClassName()
* document.getElementsByTagName()

# 修改样式

* element.style.height = '100px';
* element.setAttribute('height', '100px');
* element.setAttribute('style', 'height: 100px !important;width: 200px;')
* element.style.setProperty('height', '300px', 'important')
* element.style.cssText += 'height: 100px !important'

# 创建元素

1. 它们创建的节点只是一个孤立的节点，要通过appendChild添加到文档中
2. cloneNode要注意如果被复制的节点是否包含子节点以及事件绑定等问题
3. 使用createDocumentFragment来解决添加大量节点时的性能问题

* createElement
* createTextNode
* cloneNode
* createDocumentFragment

```html

    <ul id="myList">
        <li>Coffee</li>
        <li>Tea</li>
    </ul>
    <button onclick="myFunction()">点我</button>

    <script>
        function myFunction(){
            var node = document.createElement("button");
            var textnode = document.createTextNode("Water");
            node.appendChild(textnode);
            document.getElementById("myList").appendChild(node);
        }
    </script>
```

# 修改元素

1. 不管是新增还是替换节点，如果新增或替换的节点是原本存在页面上的，则其原来位置的节点将被移除，也就是说同一个节点不能存在于页面的多个位置
2. 节点本身绑定的事件会不会消失，会一直保留着

* appendChild
* insertBefore
* removeChild
* replaceChild

**例子：创建元素后修改样式**

```javascript
		let p = document.createElement("button")
		document.body.appendChild(p)

		setTimeout(function(){
			document.getElementsByTagName("button")[0].setAttribute("style","display:none")
		},2000)
```
