---
title: JavaScript运行机制
date: 2019-02-26 14:26:24
tags: basic
categories: JavaScript
---

**首先必须了解JS是单线程的（语言核心），最主要的原因就是为了避免DOM的（浏览器需要渲染 DOM，JS 可以修改 DOM 结构 ），虽然在HTML5中的webWorker支持多线程，但是仍然不支持对DOM的操作，所以最佳的解决方案是异步，依赖EventLoop实现。异步虽然存在很多问题，但是依然在不断完善中。**

# 进程和线程的区别

* 进程：是并发执行的程序在执行过程中分配和管理资源的基本单位，是一个动态概念，竞争计算机系统资源的基本单位（进程是一个工厂，工厂有它的独立资源，并且工厂之间相互独立）

* 线程：是进程中的一部分，一个没有线程的进程也可以被看作是单线程的，是CPU调度的一个基本单位（线程是工厂中的工人，多个工人协作完成任务，一个工厂可以有一个或者多个工人，工人之间共享空间）

进程之间的通信方式：

* 无名管道：半双工通信方式，数据只能单向流动并且只能在具有亲缘关系的进程间使用
* 有名管道：也是半双工通信方式，但是允许没有亲缘进程之间的通信
* 高级管道：将另一个程序当作一个新的程序在当前程序进程中启动，则这个进程算是当前程序的子进程
* 信号：用于通知接受进程某个事件已经发生

## js单线程的优缺点

优点：JS 是单线程运⾏的，可以达到节省内存，节约上下⽂切换时 间，没有锁的问题的好处

缺点：相对其他语言效率较低。

# 任务队列

因为Javascript是单线程的，所以意味着任务需要一个接着一个完成。但是如果前面的任务执行时间很长，那么后面的任务就得一直阻塞着，这样用户体验十分差。
JavaScript的设计者考虑到了这一点，所以他将JavaScript的任务分为两种，在主线程上执行的任务"同步任务"，被主线程挂载起来的任务"异步任务",后者一般是放在一个叫**任务队列**的数据结构中。

那么一般异步执行运行机制如下(也是JavaScript的运行机制)：

(1)所有同步任务都在主线程上执行，形成一个**执行栈**。

(2)主线程之外，还有一个“任务队列”,只要异步任务有了运行结果，就在“任务队列”之中放置一个事件。

(3)一旦“执行栈”中的所有同步任务执行完毕了，系统就会读取“任务队列”，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

(4)主线程不断重复上面的三步。（事件循环，轮询执行异步队列中的函数）

```Javascript
var test = function(){
    alert("test");
}
var test2 = function(){
    alert("test2");
}
setTimeout(function(){
   alert("setTimeout");
},1000);
test();
test2();
//test
//test2
//setTimeout;
```

# 事件和回调函数

test()和test2()属于“执行栈”中的同步任务，而**定时器则是任务队列里面的异步任务**，那么定时器就是属于异步任务中的一种，在讲定时器之前先认识一下任务队列里面的另外一个重要成员，事件。其实**任务队列就是一个事件队列**，因为一般我们绑定一个事件，比如点击事件等等，都是在某一个时刻才触发执行的，这个时候就得放到任务队列里面，等待执行，而在某个DOM节点上绑定了事件，就要有相应的回调函数,它们是相辅相成的。
所谓回调函数，就是那些被挂载起来，等待执行的代码，主线程执行任务队列里面的异步任务，其实就是执行这些回调函数。
一般只有主线程所有任务都执行完毕了，才会执行任务队列里面的异步任务，一般是按照队列的“先进先出”顺序执行，但是因为存在定时器，所以主线程要检查执行时间，只有到了规定的时间，才能返回主线程。

# 定时器

定时器主要由setTimeout()和setInterval()两个函数来完成，它们的内部运行机制完全一样，不同的只是，前者一次性执行，而后者反复执行。定时器，属于任务队列中的异步任务，所以才会出现上面的问题，再看几个例子就能理解了，

```javascript
console.log(1);

setTimeout(function(){ console.log(2);},1000);

console.log(3);
```

上面代码的执行结果是1,3,2，因为只有setTimeout里面的代码是异步任务，其它都是主线程里的同步任务，所以只有执行完了主线程中的所有任务，才会执行setTimeout中的任务。

**但是因为JS的运行机制原因，会导致定时器的运行时间不够准确**，所以可以通过时间戳的方法，使延迟的时间减去函数的运行时间。

```javascript
	<script type="text/javascript">
		var message = document.getElementById("message");
		var count = 1000;
		function animate() {
			var start = +new Date();
			message.innerHTML = count--;
			var finish = +new Date();
			setTimeout(animate, 1000 - (finish-start));
		}
    animate();
  </script>
```

## 定时器例子1

```javascript
//test1 每2秒运行一次， test2 每3秒运行一次
//求test2第二次运行是在第几秒？  
    setInterval(function test1(){
      setTimeout(function test2(){
        console.log("3")
      },3000)
      console.log("2")
    },2000)
//test1 第一次运行是在第二秒。test2 第一次运行需要等待test1 运行结束之后才能开始，所以在第五秒运行test2
//然后test2 的运行结果会被缓存，那么第二次运行就是 2+3+2 在第7秒运行，第三次运行就是2+3+2+2 在第九秒运行
```

## 定时器例子2

```javascript

    setInterval(function test1(){
      setTimeout(function test2(){
        console.log("2")
      },2000)
      console.log("5")
    },5000)
//  第一次5秒时先输出5，然后过2秒再输出2，再过3秒输出5，再过2秒输出2。525252循环下去
// test2第一次运行是在5+2 第7s，第二次运行是在5+2+5 在12秒运行
```

## 定时器例子3

```javascript

      setTimeout(function test2(){
        setInterval(function(){
          console.log("1")
        },1000)
        console.log("5")
      },5000)
//先输出5，然后再过1秒输出1，接下去循环每秒输出1
```

## 定时器例子4

```javascript

      setTimeout(function test2(){
        setInterval(function(){
          console.log("5")
        },5000)
        console.log("1")
      },1000)
//先输出1，然后再过5秒输出5，接下去循环每5秒输出5
```


# Even Loop

javascript提供的与 **“任务队列”** 有关的方法有：setTimeout、setInterval、process.nextTick和setImmediate。

process.nextTick方法可以在当前“执行栈”的尾部——下一次Event Loop(主线程读取“任务队列”)之前——触发回调函数。也就是说，它指定的任务总是发生在所有异步任务之前。setImmediate方法则是在当前“任务队列”的尾部添加事件，也即是说，它指定的任务总是在下一次Event Loop时执行。

在JavaScript中，异步任务被详细分为两种，一种宏任务（MacroTask）也叫Task，一种叫微任务（MicroTask）。

**MacroTask（宏任务）**

script全部代码、setTimeout、setInterval、setImmediate（浏览器暂时不支持，只有IE10支持，具体可见MDN）、I/O、UI Rendering。

**MicroTask（微任务）**

Process.nextTick（Node独有）、**Promise**、Object.observe(废弃)、MutationObserver

接下来介绍浏览器的JavaScript代码的具体流程：

1. 执行全局Script同步代码，这些同步代码有一些是同步语句，有一些是异步语句（比如setTimeout等）；
2. 全局Script同步代码执行完毕后，调用栈Stack会清空；
3. 从微队列microtask queue中取出位于队首的回调任务，放入调用栈Stack中执行，执行完后microtask queue长度减1；
4. 继续取出位于队首的任务，放入调用栈Stack中执行，以此类推，直到直到把microtask queue中的所有任务都执行完毕。**注意，如果在执行microtask的过程中，又产生了microtask，那么会加入到队列的末尾，也会在这个周期被调用执行；**
5. microtask queue中的所有任务都执行完毕，此时microtask queue为空队列，调用栈Stack也为空；
6. 取出宏队列macrotask queue中位于队首的任务，放入Stack中执行；
7. 执行完毕后，调用栈Stack为空；
8. 重复第3-7个步骤；
9. 重复第3-7个步骤；
10. ......

可以看到，这就是浏览器的事件循环Event Loop

举个例子：

```javascript
console.log(1);

setTimeout(() => {
  console.log(2);
  Promise.resolve().then(() => {
    console.log(3)
  });
});

new Promise((resolve, reject) => {
  console.log(4)
  resolve(5)
}).then((data) => {
  console.log(data);
})

setTimeout(() => {
  console.log(6);
})
console.log(7);
```

按照上面的步骤，将各个任务代入同步任务、宏队列和微队列中，就可以得出结果1、4、7、5、2、3、6。需要特别注意的是在这里的console.log("4")其实是同步任务。

```javascript
console.log('script start');

setTimeout(function() {
  console.log('setTimeout');
}, 0);

Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});
console.log('script end');
```

输出结果是：script start -> script end -> promise1 -> promise2 -> setTimeout

为了验证是否真的掌握还是要再举个例子：

```javascript
console.log('script start')

async function async1() {
  await async2()
  console.log('async1 end')
}
async function async2() {
  console.log('async2 end') 
}
async1()

setTimeout(function() {
  console.log('setTimeout')
}, 0)

new Promise(resolve => {
  console.log('Promise')
  resolve()
})
  .then(function() {
    console.log('promise1')
  })
  .then(function() {
    console.log('promise2')
  })

console.log('script end')
```

这里需要先理解async/await。实际上转换了

```javascript
new Promise((resolve, reject) => {
 console.log('async2 end')
 // Promise.resolve() 将代码插入微任务队列尾部
 // resolve 再次插入微任务队列尾部
 resolve(Promise.resolve())
}).then(() => {
 console.log('async1 end')
})
```

async/await 在底层转换成了 promise 和 then 回调函数。
也就是说，这是 promise 的语法糖。
每次我们使用 await, 解释器都创建一个 promise 对象，然后把剩下的 async 函数中的操作放到 then 回调函数中。
async/await 的实现，离不开 Promise。从字面意思来理解，async 是“异步”的简写，而 await 是 async wait 的简写可以认为是等待异步方法执行完成。

输出的结果是：
 script start
 async2 end
 Promise
 script end
 async1 end
 promise1
 promise2
 setTimeout

详细过程：
* 首先，打印script start，调用async1()时，返回一个Promise，所以打印出来async2 end。
* 每个 await，会新产生一个promise,但这个过程本身是异步的，所以该await后面不会立即调用。
* 继续执行同步代码，打印Promise和script end，将then函数放入微任务队列中等待执行。
* 同步执行完成之后，检查微任务队列是否为null，然后按照先入先出规则，依次执行。
* 然后先执行打印promise1,此时then的回调函数返回undefined，此时又有then的链式调用，又放入微任务队列中，再次打印promise2。
* 再回到await的位置执行返回的 Promise 的 resolve 函数，这又会把 resolve 丢到微任务队列中，打印async1 end。
* 当微任务队列为空时，执行宏任务,打印setTimeout。
这里的主要问题是把async和await弄清楚。。。

小结：执行顺序：同步代码 => 微任务(.then) => 宏任务 

# javaScript和Java、C++的区别

1. 从静态类型看还是动态类型看

* **静态类型，编译的时候就能够知道每个变量的类型，编程的时候也需要给定类型。**如Java中的整型int，浮点型float等。

* **动态类型，运行的时候才知道每个变量的类型，编程的时候无需显示指定类型，**如JavaScript中的var、PHP中的$。JavaScript、Ruby、Python都属于动态类型语言。

**静态类型还是动态类型对语言的性能有很大影响。** 

对于静态类型，在编译后会大量利用已知类型的优势，如int类型，占用4个字节，编译后的代码就可以用内存地址加偏移量的方法存取变量，而地址加偏移量的算法汇编很容易实现。 

对于动态类型，会当做字符串通通存下来，之后存取就用字符串匹配。 

2. 从编译型还是解释型来看

**编译型语言，像C、C++，在程序执行之前，有一个单独的编译过程，将程序翻译成机器语言，以后执行这个程序的时候，就不用再进行编译了。**用户只使用这些编译好的本地代码，这些本地代码由系统加载器执行，由操作系统的CPU直接执行，无需其他额外的虚拟机等。 

**解释型语言，是在运行的时候将程序翻译成机器语言，所以运行速度相对于编译型语言要慢。用户使用脚本解释器将脚本文件解释执行，没有编译过程。**对于JavaScript，随着Java虚拟机JIT技术的引入，工作方式也发生了改变。可以将抽象语法树转成中间表示（字节码），再转成本地代码，如JavaScriptCore，这样可以大大提高执行效率。但是在 V8 引擎下，⼜引⼊了 TurboFan 编译器，他会在特定的情况下进⾏优化，将代码编译成执⾏效率更⾼的 Machine Code（机器码） ，当然这个编译器并不是 JS 必须 需要的，只是为了提⾼代码执⾏性能，所以总的来说 JS 更偏向于解释型语⾔。

**Java语言，分为两个阶段。**Java语言，分为两个阶段。第一阶段，Java编译后，生成字节码，字节码与平台无关。第二阶段，在运行的时候，由JVM将字节码再翻译成机器语言。

# 为什么JS中 0.1+0.2 不等于 0.3

JS中的数字是用IEEE 754 双精度 64 位浮点数来存储的，它由64位组成,但是当**十进制小数的二进制表示**的有限数字超过 52 位时，在 JavaScript 里是不能精确存储的，超过的会被舍弃，所以存在误差。

0.1 在⼆进制中是⽆限循环的⼀些数字，其实不只是 0.1 ，其实很多⼗进制⼩数⽤⼆进制表示都是⽆限循环的。这样其实没什么 问题，但是 JS 采⽤的浮点数标准却会裁剪掉我们的数字。

* 第0位：符号位， s 表示 ，0表示正数，1表示负数；
* 第1位到第11位：储存指数部分， e 表示 ；
* 第12位到第63位：储存小数部分（即有效数字），f 表示

如果想要使得  0.1+0.2  == 0.3，在ES6中，已经为我们提供了这样一个属性：Number.EPSILON，小于这个范围内的误差则可以认为两个数字是相等的

或者 (0.1*1000+0.2*1000)/1000==0.3 //true

# Node的应⽤场景

特点：
1. 它是⼀个 Javascript 运⾏环境 
2. 依赖于 Chrome V8 引擎进⾏代码解释 
3. 事件驱动 
4. ⾮阻塞 I/O 
5. 单进程，单线程

优点：

⾼并发（最重要的优点）

缺点：
1. 只⽀持单核 CPU ，不能充分利⽤ CPU 
2. 单进程，单线程，一旦这个进程崩掉，那么整个web服务就崩掉了。

# 参考文章
[一次弄懂Event Loop（彻底解决此类面试问题）](https://juejin.im/post/5c3d8956e51d4511dc72c200)
[带你彻底弄懂 Event Loop](https://segmentfault.com/a/1190000016278115?utm_source=tag-newest#articleHeader8)