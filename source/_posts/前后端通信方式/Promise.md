---
title: Promise
date: 2019-4-29 10:12:25
tags: basic
categories: Ajax
---

# promise 对象
 
![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/promise.png)

概念：Promise是异步编程的一种解决方案，比传统的解决方案（回调函数和事件）更合合理、强大。所谓Promise，简单来说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说**Promise是一个对象，从它可以获取异步操作的消息**。Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。将异步操作以同步操作的流程表达出来，**避免了层层嵌套的回调函数**，解决了JQuery的回调地狱问题。

它有三种状态，分别是**pending-进行中、resolved-已完成、rejected-已失败。**
另外， fulfilled 与 rejected ⼀起合称 settled。

可以把 Promise 看成一个状态机。初始是 pending 状态，可以通过函数 resolve 和reject，将状态转变为 resolved 或者 rejected 状态，状态一旦改变就不能再次变化
then 函数会返回一个 Promise 实例，并且该返回值是一个**新的实例**而不是之前的实例。因为Promise 规范规定除了 pending 状态，其他状态是不可以改变的，如果返回的是一个相同实例的话，多个 then调用就失去意义了

但是Promise也存在以下缺点：

* Promise一旦创建就会立即执行，无法中途取消
* 当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）
* 如果不设置回调函数，Promise内部抛出的错误不会反应到外部


```JavaScript
var promise = new Promise((resolve, reject) => {
    var success = true;
    if (success) {
        resolve('成功');
    } else {
        reject('失败');
    }
}).then(
    (data) => { console.log(data)},
    (data) => { console.log(data)}
)
```

分析**promise的执行过程**

```JavaScript

setTimeout(function() {
    console.log(0);
}, 0);
var promise = new Promise((resolve, reject) => {
    console.log(1);
    setTimeout(function () {
        var success = true;
        if (success) {
            resolve('成功');
        } else {
            reject('失败');
        }
    },2000);
}).then(
    (data) => { console.log(data)},
    (data) => { console.log(data)}
);

console.log(promise);	//<pending> 进行中
setTimeout(function () {
    console.log(promise);	//<resolved> 已完成
},2500);
console.log(2);
 
//1  
//Promise {<pending>}
//2
//0
//成功
//Promise {<resolved>: undefined}
```

# promise 原理

promise就是三个状态。利用观察者模式的编程思想,通过特定书写方式注册对应状态的事件处理函数，然后更新状态，调用注册过的处理函数即可。 
这个特定方式就是then，done，fail，always…等方法，更新状态就是resolve、reject方法。 
 
Promise的实现过程，通过Promise.prototype.then和Promise.prototype.catch方法将观察者方法注册到被观察者Promise对象中，同时返回一个新的Promise对象，以便可以链式调用。

被观察者管理内部pending、fulfilled和rejected的状态转变，同时通过构造函数中传递的resolve和reject方法以主动触发状态转变和通知观察者。

```javascript
/**
 * Promise类实现原理
 * 构造函数传入一个function，有两个参数，resolve：成功回调; reject：失败回调
 * state: 状态存储 [PENDING-进行中 RESOLVED-成功 REJECTED-失败]
 * doneList: 成功处理函数列表
 * failList: 失败处理函数列表
 * done: 注册成功处理函数
 * fail: 注册失败处理函数
 * then: 同时注册成功和失败处理函数
 * always: 一个处理函数注册到成功和失败
 * resolve: 更新state为：RESOLVED，并且执行成功处理队列
 * reject: 更新state为：REJECTED，并且执行失败处理队列
**/
    class PromiseNew {
    constructor(fn) {
        this.state = 'PENDING';
        this.doneList = [];
        this.failList = [];
        fn(this.resolve.bind(this), this.reject.bind(this));
    }
    // 注册成功处理函数
    done(handle) {
        if (typeof handle === 'function') {
        this.doneList.push(handle);
        } else {
        throw new Error('缺少回调函数');
        }
        return this;
    }
    // 注册失败处理函数
    fail(handle) {
        if (typeof handle === 'function') {
        this.failList.push(handle);
        } else {
        throw new Error('缺少回调函数');
        }
        return this;
    }
    // 同时注册成功和失败处理函数
    then(success, fail) {
        this.done(success || function () { }).fail(fail || function () { });
        return this;
    }
    // 一个处理函数注册到成功和失败
    always(handle) {
        this.done(handle || function () { }).fail(handle || function () { });
        return this;
    }
    // 更新state为：RESOLVED，并且执行成功处理队列
    resolve() {
        this.state = 'RESOLVED';
        let args = Array.prototype.slice.call(arguments);
        setTimeout(function () {
        this.doneList.forEach((item, key, arr) => {
            item.apply(null, args);
            arr.shift();
        });
        }.bind(this), 200);
    }
    // 更新state为：REJECTED，并且执行失败处理队列
    reject() {
        this.state = 'REJECTED';
        let args = Array.prototype.slice.call(arguments); //arrayObject.slice(start,end)slice() 方法可从已有的数组中返回选定的元素。
        setTimeout(function () {
        this.failList.forEach((item, key, arr) => {
            item.apply(null, args);
            arr.shift(); // shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。
        });
        }.bind(this), 200);
      }
    }
    
    // 下面一波骚操作
    new Promise((resolve, reject) => {
      resolve('hello world');
      // reject('you are err');
      }).done((res) => {
        console.log(res);
      }).fail((res) => {
        console.log(res);
    })
```

# promise 常用API

1. Promise.resolve(value)

类方法，该方法返回一个以 value 值解析后的 Promise 对象

* 如果这个值是个 thenable（即带有 then 方法），返回的 Promise 对象会“跟随”这个 thenable 的对象，采用它的最终状态（指 resolved/rejected/pending/settled）
* 如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回
* 其他情况以该值为成功状态返回一个 Promise 对象

```javascript

    //如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。  
    function fn(resolve){
        setTimeout(function(){
            resolve(123);
        },3000);
    }
    let p0 = new Promise(fn);
    let p1 = Promise.resolve(p0);
    // 返回为true，返回的 Promise 即是 入参的 Promise 对象。
    console.log(p0 === p1);

```

2. Promise.reject

类方法，且与 resolve 唯一的不同是，返回的 promise 对象的状态为 rejected

3. Promise.prototype.then

实例方法，为 Promise 注册回调函数，函数形式：fn(vlaue){}，value 是上一个任务的返回结果，then 中的函数一定要 return 一个结果或者一个新的 Promise 对象，才可以让之后的then 回调接收

4. Promise.prototype.catch

实例方法，捕获异常，函数形式：fn(err){}, err 是 catch 注册 之前的回调抛出的异常信息。

5. Promise.race

类方法，多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败

6. Promise.all

类方法，多个 Promise 任务同时执行。
如果全部成功执行，则以数组的方式返回所有 Promise 任务的执行结果。 如果有一个 Promise 任务 rejected，则只返回 rejected 任务的结果

# promise的基本使用

有以下场景，需要有3个异步操作，分别是buy、cook和send，三个步骤都是需要消耗时间的，可以理解为三个异步任务,有另外一个同步任务就是call

```javascript

     function buy(resolve){
      resolve()
      console.log("买菜")
     }

     function cook(resolve){
      resolve()
      console.log("做饭")
     }

     function send(resolve){
      resolve()
      console.log("送饭")
     }
     
     function call(resolve){
         console.log("通知完成")
     }

     new Promise(buy)
     .then((结果1)=>{
      return new Promise(cook)
     })
     .then((结果2)=>{
      return new Promise(send)
     })
     .then((结果3)=>{
         call()
     })
```

如果我们的后续任务是异步任务的话，必须return 一个 新的 promise 对象
如果后续任务是同步任务，只需 return 一个结果即可

```javascript
//上面例子改写为
  let promise = new Promise((resolve,reject) =>{
    var success = true
    if(success){
      resolve()
      console.log("买菜")
    }else{
      reject()
    }
  })
  promise
  .then(() => {
      console.log("做饭")
  })
  .then(() =>{
    console.log("送饭")
  })
  .then(() =>{
    console.log("完成")
  })
```

# async 和 await

谈及异步回调函数的嵌套，总会让人感到烦躁，特别是当业务逻辑复杂，往往需要调用几次 ajax 才能拿到所有需要的数据。

从最早的回调函数，到 Promise 对象，再到 Generator 函数，每次都有所改进，但又让人觉得不彻底。它们都有额外的复杂性，都需要理解抽象的底层运行机制。所以，我们需要一种方法，更优雅地解决异步操作。于是，async函数出现了。

一句话解释：async 函数，就是 Generator 函数的语法糖。

async 函数具有以下特点：

* 建立在 promise 之上。所以，不能把它和回调函数搭配使用。但它会声明一个异步函数，并隐式地返回一个 Promise。因此可以直接 return 变量，无需使用 Promise.resolve 进行转换。
* 和 promise 一样，是非阻塞的。但不用写 then 及其回调函数，这减少代码行数，也避免了代码嵌套。而且，所有异步调用，可以写在同一个代码块中，无需定义多余的中间变量。
* 它的最大价值在于，可以使异步代码，在形式上，更接近于同步代码。
它总是与 await 一起使用的。并且，await 只能在 async 函数体内。
* await 是个运算符，用于组成表达式，它会阻塞后面的代码。如果等到的是 Promise 对象，则得到其 resolve值。否则，会得到一个表达式的运算结果。

## async函数的优点

```javascript
    function takeLongTime(n) {
        return new Promise(resolve => {
            setTimeout(() => resolve(n + 200), n);
        });
    }
    function step1(n) {
        console.log(`step1 with ${n}`);
        return takeLongTime(n);
    }
    function step2(n) {
        console.log(`step2 with ${n}`);
        return takeLongTime(n);
    }
    function step3(n) {
        console.log(`step3 with ${n}`);
        return takeLongTime(n);
    }
```

使用 Promise 方法实现三个步骤的处理：

```javascript
    function doIt() {
        console.time("doIt");
        const time1 = 300;
        step1(time1)
            .then(time2 => step2(time2))
            .then(time3 => step3(time3))
            .then(result => {
                console.log(`result is ${result}`);
            });
    }
    doIt();
    // step1 with 300
    // step2 with 500
    // step3 with 700
    // result is 900
```

使用async处理：

```javascript
    async function doIt() {
        console.time("doIt");
        const time1 = 300;
        const time2 = await step1(time1);
        const time3 = await step2(time2);
        const result = await step3(time3);
        console.log(`result is ${result}`);
    }
    doIt();
```

async相比promise优点：

1. 使用Async/Await明显节约了不少代码。

我们不需要写.then，不需要写匿名函数处理Promise的resolve值，也不需要定义多余的data变量，还避免了嵌套代码。这些小的优点会迅速累计起来，这在之后的代码示例中会更加明显。

2. 相比于 Promise 更易于调试。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/async1.png)
因为没有代码块，所以不能在一个返回的箭头函数中设置断点。如果你在一个 .then 代码块中使用调试器的步进(step-over)功能，调试器并不会进入后续的 .then 代码块，因为调试器只能跟踪同步代码的每一步。

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/async2.png)
现在，如果使用 async/await，你就不必再使用箭头函数。你可以对 await 语句执行步进操作，就好像他们都是普通的同步语句一样。

3. 更好的中间值处理

你很可能遇到过这样的场景，调用promise1，使用promise1返回的结果去调用promise2，然后使用两者的结果去调用promise3。你的代码很可能是这样的:

```javascript
const makeRequest = () => {
    return promise1().then(value1 => {
        return promise2(value1).then(value2 => {
            return promise3(value1, value2)
        })
    })
}

```

如果promise3不需要value1，可以很简单地将promise嵌套铺平。如果你忍受不了嵌套，你可以将value 1 & 2 放进Promise.all来避免深层嵌套：

```javascript
const makeRequest = () => {
    return promise1().then(value1 => {
        return Promise.all([value1, promise2(value1)])
        }).then(([value1, value2]) => {
            return promise3(value1, value2)
        })
}
```

这种方法为了可读性牺牲了语义。除了避免嵌套，并没有其他理由将value1和value2放在一个数组中。

使用async/await的话，代码会变得异常简单和直观。

```javascript

const makeRequest = async () => {
    const value1 = await promise1()
    const value2 = await promise2(value1)
    return promise3(value1, value2)
}

```

但是缺点在于滥⽤ await 可能会导致性能问 题，因为 await 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然 需要等待前者完成，导致代码失去了并发性。

# async的基本使用

基本使用：

```javascript

	function timeout(ms){
		return new Promise(resolve =>{
			setTimeout(resolve,ms)
		})
	}

	async function print(value,ms){
		await timeout(ms)
		console.log(value)
	}

	print('hello',1000)
	print('world',1000)
```

实现链式调用：

```javascript

function delay(){
	return new Promise((resolve) => {
		setTimeout(()=> {
		    resolve ()
		},Math.random()*3000);
	})
}


function fg(name){
	return function(){
		return delay().then(()=>{
			console.log(name)
		})
	}
}

const f1 = fg('f1')
const f2 = fg('f2')
const f3 = fg('f3')
const f4 = fg('f4')

async function print(){
	await f1()
	await f2()
	await f3()
	await f4()
}
print()
```

# 用Promise对象实现的 Ajax

```javascript
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    
    const xhr = new XMLHttpRequest()
    xhr.open("GET", url)
    xhr.onreadystatechange = handler
    xhr.responseType = "json"
    xhr.setRequestHeader("Accept", "application/json")
    xhr.send()

    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }

      
      if (this.status === 200) {
        resolve(this.response)
      } else {
        reject(new Error(this.statusText));
      }
    }
  })

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});

```

# Promise结合定时器实现动画效果

```HTML
    .aaa{
        background: red;
        height: 100px;
        width: 100px;
    }
		
	<div class="aaa"></div>

    <script>
    // 写法一：
        var changeColor = document.getElementsByClassName('aaa')
		const change = new Promise(function(resolve,reject){
			resolve("312312")
		})

		change
        .then(function(){
			setTimeout(function(){
				changeColor[0].setAttribute('style', 'background: yellow !important;height:200px;');
			},1000)
		})
		.then(function(){
			setTimeout(function(){
				changeColor[0].setAttribute('style', 'background: black;height:200px;');
			},1000)
		})
    </script>
```



# 参考文章：

[Promise原理和问题集锦](https://blog.csdn.net/juse__we/article/details/88900685)