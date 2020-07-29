---
title: javaScript中的this指向
date: 2019-03-21 16:12:22
tags: Interview
categories: JavaScript
---

# this指向

this的指向有4种类型

## 形式"test()"

```javascript

var a = 1
function test () {
    console.log(this.a)
}
test()

```

直接不带任何引用形式去调用函数，则this会指向全局对象，因为没有其他影响去改变this，this默认就是指向全局对象（浏览器是window，Node中是global）的。这个结论是在非严格模式的情况下，严格模式下这个this其实是undefined的。

## 形式"xxx.text()"

```javascript

var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
obj.test()

```
这种形式相对于第一中，this指向已经很明显，谁去调用这个函数的，这个函数中的this就绑定到谁身上。


```JavaScript

var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
var obj0 = {
    a: 3,
    obj 
}
obj0.obj.test()
```

虽然比上面复杂了一点，但是结果依然和上面的一样，this指向obj，指向直接调用的对象,结果依然是2。

```JavaScript

var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
var testCopy = obj.test
testCopy()
```
道理同上，虽然说换了一个名字，但是调用的是window，所以结果是1。


```JavaScript

var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
setTimeout(obj.test)  //输出1

```

## 形式“test.call(xxx) / test.apply(xxx) / test.bind()”

```JavaScript
var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
var testCopy = obj.test
testCopy.call(obj)//2
```

可以看到，我们通过call（apply跟call的区别只是传参，作用是一样的，bind有点区别，bind能让我们的函数延迟执行，apply与call调用就执行，所以bind这样的形式我们也称为函数柯里化，这些就不是我们这里要说的啦）来调用testCopy，并且传入了你想要this指向的上下文，那么this指向你要的对象。看到这里，我们也可以想象第一、二种形式其实可以转化成call/apply的形式

## “new test()”形式

```JavaScript

var a = 1
function test (a) {
    this.a = a
}
var b = new test(2)
console.log(b.a)

```

new这个操作符其实是new了一个新对象出来，而被new的test我们称为构造函数，我们可以在这个构造函数里定义一下将要到来的新对象的一些属性。所以构造函数里的this指的就是将要被new出来的新对象。

## 箭头函数中的this指向

**箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定**所谓的定义时候绑定，就是this是继承自父执行上下文中的this！！

```JavaScript
var a = 1
var test = () => {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
obj.test()      //  输出1
```

**它this指向的是它的外层作用域this的指向。**。外层作用域是整个window，所以输出的是1.

**注意：简单对象（非函数）是没有执行上下文的！**

### 改变箭头函数的this的指向

1. 使用双冒号::进行绑定,但是该方法目前只是在ES7的提案中，虽然babel转码器已经支持但是仍未成为标准，所以在实际使用中可能会报错

```javascript

//双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象作为上下文环境绑定到右边的函数上
foo::bar
//相当于
bar.bind(foo)

```

2. 使用call

```javascript

	var name = "jack"
	var obj = {
		name: "nick"
	}
	function foo(){
	 	setTimeout(()=>{
	 		console.log(this.name)
	 	},100)
	}
	foo()       //  输出jack
	foo.call(obj)       //  输出nick

```

箭头函数导致this总是指向函数定义生效时所在的对象。上面的call改变了this的执行上下文，所以指向了obj

箭头函数可以让setTimeout里面的this绑定定义时所在的作用域，而不是指向运行时所在的作用域

```javascript

	function Timer(){
		this.s1 = 0
		this.s2 = 0
		setInterval(() => this.s1 ++ ,1000)
		setInterval(function(){
			this.s2++
		},1000)
	}
	var timer = new Timer()
	setTimeout(() => console.log(timer.s1),3100)		//s1: 3
	setTimeout(() => console.log(timer.s2),3100)		//s2: 0

// 前者的this绑定定义时所在的作用域，即Timer函数，tihs.s1更新了3次
// 后者运行时的this指向全局对象，this.s2更新0次

```

## apply、call 和 bind 的作用。

apply和call、bind的目的就是为了改变函数内部this的指向。

除了第⼀个参数外， call 可以接收⼀个参数列表， apply 只接受⼀个参数数组。bind是es5的语法。

```javascript
    var name = 'global';
    var person = {
      name: 'zero'
    };

  function printInfo(age, job) {
      console.log(this.name, age, job);
  }

    printInfo(20, '前端工程师');        // 打印：global 20 前端工程师,因为默认的上下文是window，所以this.name是全局定义的global，如果我们想打印出来zero的话，就需要改变函数执行的上下文

    printInfo.call(person, 20, '前端工程师');// zero 20 前端工程师
    printInfo.apply(person, [20, '前端工程师']);// zero 20 前端工程师
// 这两种方式是一样的，第一个参数都是传进去的上下文，this.name取的是person.name，所以打印出来的名字就是zero了，后面的为age和job，只是参数传递的方式不一样，apply比较特殊，把要传的参数放在数组里面

// 而bind和以上两种有区别，bind是es5定义的新方法，用来返回一个有自己上下文的函数，用法也比较类似：
    printInfo.bind(person)(20, '前端工程师');// zero 20 前端工程师
// printInfo.bind(person)这一块是返回的以peron为上下文的函数，后面的(20, '前端工程师')是函数调用
```

bind后函数不会立即执行，而只是返回一个改变了上下文的函数副本，而call和apply是直接执行函数，所以我们可以通过 bind 实现柯⾥化


## apply、call 和 bind 的应用

1. 数组之间的追加(使用a.concat(b) 会返回一个新的数组，不会影响原来的数组)

```javascript
let a = [4,5,6]
let b = [7,8,9]

Array.prototype.push.apply(a, b)

console.log(a)    //[4, 5, 6, 7, 8, 9]
console.log(b)    // [7, 8, 9]

```

2. 获取数组中的最大值和最小值，利用他们扩充作用域拥有Math的min和max方法

由于没有什么对象调用这个方法，所以第一个参数可以写作null或者本身

```javascript

var  numbers = [5, 458 , 120 , -215 ]

var  maxInNumbers = Math.max.apply(Math, numbers),   //458   

// maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
// console.log(Math.max([5, 458 , 120 , -215 ]))

```

3. 验证是否是数组（前提是toString（）方法没有被重写过）

```javascript

Object.prototype.toString.call(fn) // "[object Function]"  函数

var date = new Date()
Object.prototype.toString.call(date) // "[object Date]"    日期 

var arr = [1,2,3]
Object.prototype.toString.call(arr) // "[object Array]"    数组

var reg = /[hbc]at/gi;
Object.prototype.toString.call(reg) // "[object RegExp]"   正则

```

4. 让伪数组拥有数组的方法

* Array.prototype.slice.call()
* [].prototype.slice.call()
* Array.from()
* 扩展运算符[...()]
