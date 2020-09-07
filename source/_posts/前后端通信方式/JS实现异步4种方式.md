---
title: JavaScript异步的四种方式
date: 2019-5-12 11:12:25
tags: basic
categories: Ajax
---

# 使用 callback 回调函数

优点：简单、容易理解
缺点：不利于维护，代码耦合⾼

```javascript
    function f1 (callback) {
        setTimeout(function () {
    　　　　 console.log("1")
            callback()
        },2000)
    }

    function f2(){
        console.log("2")
    }

    f1(f2)    //过了2秒之后再输出1，2
```

缺点会出现回调地狱。

# Promise(ES6)

优点：可以利⽤then⽅法，进⾏链式写法；可以书写错误时的回调函数
缺点：编写和理解，相对⽐较难

* Promise对象代表一个异步操作，有三种状态：pending(进行中),resolved(已成功)和rejected(已失败)。

* 一旦状态改变，就不会再变，任何时候都可以得到这个结果。状态改变只有两种可能：pending=>resolved,pending=>rejected。

```javascript

const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value)
  } else {
    reject(error)
  }
})

```

Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

```javascript

    promise.then(function(value) {
    // success
    }, function(error) {
    // failure
    })
```

then方法接收两个回调函数，第一个回调函数是Promise对象的状态变为resolved时调用。第二个回调函数是Promise对象的状态变为rejected时调用。

缺点是：

* 无法取消Promise，一旦新建就立即执行，无法中途取消。
* 不设置回调函数，Promise内部抛出的错误，不会反应到外部。
* 当初与pending状态时，无法得知目前进展到哪一个阶段(刚刚开始还是即将完成)。

# Generator(ES6)

优点：函数体内外的数据交换、错误处理机制
缺点：流程管理不⽅便

```javascript

    function* G() {
        const a = yield 100
        console.log('a', a)  // a aaa
        const b = yield 200
        console.log('b', b)  // b bbb
        const c = yield 300
        console.log('c', c)  // c ccc
    }
    const g = G()
    g.next()    // value: 100, done: false
    g.next('aaa') // value: 200, done: false
    g.next('bbb') // value: 300, done: false
    g.next('ccc') // value: undefined, done: true

```

相比Promise，Generator流程更加直观、语义化。
Generator的问题在于，函数的执行需要执行器每次都需要通过g.next()方式执行。

# async/await(ES2017)

优点：内置执⾏器、更好的语义、更⼴的适⽤性、返回的是Promise、结构清晰。
缺点：错误处理机制

```javascript

    async function testResult() {
        let first = await doubleAfter2seconds(30);
        let second = await doubleAfter2seconds(50);
        let third = await doubleAfter2seconds(30);
        console.log(first + second + third);
    }

```

asnyc函数解决了上述问题，流程清晰、直观、语义明显。
同时asnyc函数自带执行器，执行的时候无需手动加载。

## 如何处理async和await的错误

1. 使用try...catch捕获

```javascript
function fetchA(){
  return new Promise(resolve,reject){
    ...
  }
}

function fetchB(){
  return new Promise(resolve,reject){
    ...
  }
}

async function fn() {
  // 每个异步都需要处理
  try {
    let res = await fetchA()
    console.log(res)
  } catch (error) {
    console.log(error);
  }
  try {
    let res = await fetchB()
    console.log(res)
  } catch (error) {
    console.log(error)
  }
}
fn()
```

但是这样写代码里充斥着 try/catch，但是如果只是使用一个 try/catch，如下所示，当捕获到多个错误的时候，要判断错误类型的话只能再定义err的类型，反而增加了编码的难度

```javascript
async function fn() {
  // 每个异步都需要处理
  try {
    let resA = await fetchA()
    let resB = await fetchB()
    console.log(resA,resB)
  } catch (error) {
   if (error.type === 'resA') {
   console.log('resA err is', err)
   }
   if (error.type === 'resB') {
   console.log('resB err is', err)
   }
  }
}
fn()
```

2. 因为 asycn...await 的本质就是 promise 的语法糖因此可以使用 then 函数，然后再将 try...catch 抽离

```javascript
//核心思路借鉴 await-to-js

/* 方法抽取 */
const to = promise => {
  return promise.then(res => [null, res]).catch(error => [error])
}

let flag = true
//异步操作
function fetchVehicle() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (flag) {
        resolve(true)
      } else {
        reject(false)
      }
    }, 1000)
  })
}

async function fn() {
  // 每个异步都需要处理
  let [error, res] = await to(fetchVehicle())
  console.log("error", error)
  console.log("res", res)
}

fn()
```

# 事件监听(采⽤时间驱动模式，取决于某个事件是否发⽣)

优点：容易理解，可以绑定多个事件，每个事件可以指定多个回调函数。
缺点：事件驱动型，流程不够清晰。

# 发布/订阅(观察者模式)

类似于事件监听，但是可以通过‘消息中⼼ʼ，了解现在有多少发布者，多少订阅者。