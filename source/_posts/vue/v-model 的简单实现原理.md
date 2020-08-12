---
title: v-model简单实现原理
date: 2019-05-22 14:43:20
tags: basic
categories: Vue

---
写在前面：上网查资料的时候，发现了《Vue.js权威指南这本书》，作者张耀春人品有问题，真的是被恶心到了，所以先入为主，这书的具体内容就看也不看了。讲道理vue好像没有一本口碑比较好的作品。还是看官方文档好了。
官网实例：

# 实现双向绑定

```html
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```
实现双向数据绑定，v-model只是一个语法糖而已，真正的实现，其实靠的是v-bind绑定响应式的数据，然后触发input事件进行数据的传递。
对上面的例子进行重构
```html
<input v-bind:value="msg" v-on:input="msg=$event.target.value">
```

原理：主要还是通过**Object对象的defineProperty属性，重写data的set和get函数来实现**
下面是一个非常简陋的一个demo
```javaScript
    <input type="text" name="数据绑定" id="dataBinding">
    <span id="data"></span><br>

mounted() {
    this.showData()
},
methods:{
      showData(){
          var obj1 = {};                        //新建一个对象obj1
          Object.defineProperty(obj1,'hello',{  //为obj定义一个名为 hello 的访问器属性 
              set: function(newVal){
                  document.getElementById('dataBinding').value = newVal;
                  document.getElementById('data').innerHTML = newVal;
              },
              get: function(){
                  return
              }
          });
          document.addEventListener('keyup',function(e){
              obj1.hello = e.target.value;
          });
          console.log(obj1);
      }
}

```

# 基于Proxy实现双向绑定todolist Demo

```html

 <div id="app">
      <input type="text" id="input" />
      <div>您输入的是： <span id="title"></span></div>
      <button type="button" name="button" id="btn">添加到todolist</button>
      <ul id="list"></ul>
 </div>

```

```javascript

    const obj = {};
    const input = document.getElementById("input");
    const title = document.getElementById("title");
    
    const newObj = new Proxy(obj, {
      get: function(target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
      },
      set: function(target, key, value, receiver) {
        console.log(target, key, value, receiver);
        if (key === "text") {
          input.value = value;
          title.innerHTML = value;
        }
        return Reflect.set(target, key, value, receiver);
      }
    });

    input.addEventListener("keyup", function(e) {
      newObj.text = e.target.value;
    });


     // 渲染todolist列表
    const Render = {
      // 初始化
      init: function(arr) {
        const fragment = document.createDocumentFragment();
        for (let i = 0; i < arr.length; i++) {
          const li = document.createElement("li");
          li.textContent = arr[i];
          fragment.appendChild(li);
        }
        list.appendChild(fragment);
      },
      addList: function(val) {
        const li = document.createElement("li");
        li.textContent = val;
        list.appendChild(li);
      }
    };

    //
    const arr = [];
    // 监听数组
    const newArr = new Proxy(arr, {
      get: function(target, key, receiver) {
        return Reflect.get(target, key, receiver);
      },
      set: function(target, key, value, receiver) {
        console.log(target, key, value, receiver);
        if (key !== "length") {
          Render.addList(value);
        }
        return Reflect.set(target, key, value, receiver);
      }
    });

    // 初始化
    window.onload = function() {
      Render.init(arr);
    };

    btn.addEventListener("click", function() {
      newArr.push(parseInt(newObj.text));
    });
```