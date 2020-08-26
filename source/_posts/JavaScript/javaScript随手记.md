---
title: JavaScript随手记
date: 2019-01-26 16:16:24
tags: Interview
categories: JavaScript
---

# 一、作用域

## 1、作用域概念

作用域：变量可以起作用的范围

全局变量：使用var声明的，在任何地方都可以访问到的变量就是全局变量，对应全局作用域，如果页面不关闭，则内存不会释放，一直占用空间。

局部变量：只在固定的代码片段内可访问到的变量，最常见的例如函数内部。对应局部作用域(函数作用域)。

隐式全局变量：声明的变量没有var,就叫隐式全局变量。

不使用var声明的变量是全局变量，不推荐使用。

```javascript
   function f1() {
     number=1000;//是隐式全局变量
   }
   f1();
   console.log(number);
```

## 2、块级作用域

任何一对花括号（｛和｝）中的语句集都属于一个块，在这之中定义的所有变量在代码块外都是不可见的，我们称之为块级作用域。在es5之前没有块级作用域的的概念,只有函数作用域。

## 3、 js 中词法作用域规则:

函数允许访问函数外的数据.

整个代码结构中只有函数可以限定作用域.

作用域规则首先使用提升规则分析

如果当前作用规则中有名字了, 就不考虑外面的名字

## 4、作用域链

只有函数可以制造作用域结构， 那么只要是代码，就至少有一个作用域, 即全局作用域。凡是代码中有函数，那么这个函数就构成另一个作用域。如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域。

将这样的所有的作用域列出来，可以有一个结构: 函数内指向函数外的链式结构。就称作作用域链。

```javascript
function f1() {
    function f2() {
        function f3() {
    	}
    }
}
```

# 二、预解析

JavaScript代码的执行是由浏览器中的**JavaScript解析器**来执行的。JavaScript解析器执行JavaScript代码的时候，分为两个过程：**预解析过程和代码执行**过程。预解析就是在全局中寻找var关键字声明的变量和通过function关键字声明的函数。
预解析过程：

1. 把变量的声明提升到当前作用域的最前面，只会提升声明，不会提升赋值。
2. 把函数的声明提升到当前作用域的最前面，只会提升声明，不会提升调用。
3. 先提升函数，然后再提升变量。
4. 只有变量和函数才会发生声明提升，并且变量在提升声明的时候不会赋值，默认值是undefined，函数声明也不会将函数调用。
5. 通过声明提升，函数可以在声明的函数体之上进行调用，变量也可以在赋值之前进行输出，而且不会报错，如果说直接输出一个没有声明的变量就会报错。

```javascript
f1(); // 'hello'
function f1() {
    console.log('hello');
} // 此时预解析函数 f1 声明提升, 所以上面的调用控制台中调用不会报错
console.log(num); // undefined  此时预解析变量 num 声明提升（没有赋值）
var num = 5; //  所以上面在控制台中调用不会报错，只是显示 undefined
```

# 三、this指向

## 1、形式"test()""

```JavaScript
var a = 1
function test () {
    console.log(this.a)
}
test()  //1
```
直接不带任何引用形式去调用函数，则this会指向全局对象，因为没有其他影响去改变this，this默认就是指向全局对象（浏览器是window，Node中是global）的。这个结论是在非严格模式的情况下，严格模式下这个this其实是undefined的。

## 2、形式"xxx.text()"

```JavaScript
var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
obj.test()      //2
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
obj0.obj.test()     //2
```
虽然比上面复杂了一点，但是结果依然和上面的一样，this指向obj，指向直接调用的对象。

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
testCopy()      //testCopy()是由window调用的，所以this指向window
```
道理同上，虽然说换了一个名字，但是只认定函数调用的时候，是由哪个对象进行调用的。


```Javascript
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
## 3、形式“test.call(xxx) / test.apply(xxx) / test.bind()”

```JavaScript
var a = 1
function test () {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
var obj3 = {
    a: 3,
    test
}
var testCopy = obj.test
testCopy.call(obj)//2
```
可以看到，我们通过 call（apply 跟 call 的区别只是传参，作用是一样的，bind 有点区别，bind 能让我们的函数延迟执行，apply 与 call 调用就执行，所以 bind 这样的形式我们也称为函数柯里化，这些就不是我们这里要说的啦）来调用 testCopy，并且传入了你想要 this 指向的上下文，那么 this 指向你要的对象。看到这里，我们也可以想象第一、二种形式其实可以转化成 call/apply 的形式。

## 4、“new test()”形式

```
var a = 1
function test (a) {
    this.a = a
}
var b = new test(2)
console.log(b.a)        //2
```
new这个操作符其实是new了一个新对象出来，而被new的test我们称为构造函数，我们可以在这个构造函数里定义一下将要到来的新对象的一些属性。所以构造函数里的this指的就是将要被new出来的新对象。

最后的最后，其实还有一个，就是箭头函数中的this指向。。
**箭头函数中的this是在定义函数的时候绑定，而不是在执行函数的时候绑定**所谓的定义时候绑定，就是this是继承自父执行上下文中的this！！

```
var a = 1
var test = () => {
    console.log(this.a)
}
var obj = {
    a: 2,
    test
}
obj.test()
```
**它this指向的是它的外层作用域this的指向。**。外层作用域是整个window，所以输出的是1.

# 四、typeof和instanceof的区别

## typeof

typeof 是一个一元运算，放在一个运算数之前，运算数可以是任意类型。**它返回值是一个字符串，该字符串说明运算数的类型。**

typeof 一般只能返回如下几个结果：
"number"、"string"、"boolean"、"object"、"function" 和 "undefined"以及"symbol"。

```
运算数为数字 typeof(x) = "number" 

字符串 typeof(x) = "string" 

布尔值 typeof(x) = "boolean" 

对象,数组和null typeof(x) = "object" 

函数 typeof(x) = "function"
```

## instanceof
instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

```
var a=new Array();

alert(a instanceof Array); // true，

//同时 alert(a instanceof Object) //也会返回 true;

//这是因为 Array 是 object 的子类。
```

另外，更重的一点是 instanceof 可以在继承关系中用来判断一个实例是否属于它的父类型。

```javaScript
function Foo(){} 
Foo.prototype = new Aoo();//JavaScript 原型继承 
var foo = new Foo(); 
console.log(foo instanceof Foo)//true 
console.log(foo instanceof Aoo)//true
```

# 五、null和undefined 区别
null： Null类型，代表“空值”，代表一个**空对象指针**，使用typeof运算得到 **“object”**，所以可以认为它是一个特殊的对象值。

undefined： Undefined类型，声明但是未初始化变量时，得到的就是undefined。

null是javascript的关键字，可以认为是对象类型，它是一个空对象指针，和其它语言一样都是代表“空值”，不过 undefined 却是javascript才有的。undefined是在ECMAScript第三版引入的，为了区分空指针对象和未初始化的变量，它是一个预定义的全局变量。没有返回值的函数返回为undefined，没有实参的形参也是undefined。

也可以认为undefined是表示系统级的、出乎意料的或类似错误的值的空缺，而null是表示程序级的、正常的或在意料之中的值的空缺