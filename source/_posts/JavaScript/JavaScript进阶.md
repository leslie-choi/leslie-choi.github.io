---
title: javaScript高级（一）
date: 2019-02-19 14:16:24
tags: Interview
categories: JavaScript
---
# JavaScript高级

先来重新再认识一下JavaScript吧。JavaScript，其实是一门脚本、弱类型、轻量级的解释性语言。
针对解释性语言，不同于编译性语言。编译性语言编写的程序，在执行之前需要有一个专门的编译过程，把程序编译成机器语言的文件，比如exe。然后exe文件运行的时候，直接使用编译的结果就可以了，运行的时候不用再继续编译，所以**执行效率高。具有独立性。**
而对于解释性语言，比如JavaScript，是**非独立的**，运行需要依赖环境，对于客户端是浏览器，对于服务端是node。
**效率低**，执行前不需要编译，执行的时候才编译，因此效率也低。

## JavaScript的执行过程
JavaScript 运行分为两个阶段：

- 预解析
  + 全局预解析（所有变量和函数声明都会提前；同名的函数和变量函数的优先级高）
  + 函数内部预解析（所有的变量、函数和形参都会参与预解析）
    * 函数
    * 形参
    * 普通变量
- 执行

先预解析全局作用域，然后执行全局作用域中的代码，
在执行全局代码的过程中遇到函数调用就会先进行函数预解析，然后再执行函数内代码。

## apply和call方法的使用

apply和call作用是都可以**改变函数执行时的上下文** ，也就是说可以改变当前this的指向。apply和call方法也是函数的调用的方式。
**apply的使用语法**
​    函数名字.apply(对象,[参数1,参数2,...]);
​    方法名字.apply(对象,[参数1,参数2,...]);
**call的使用语法**
​    函数名字.call(对象,参数1,参数2,...);
​    方法名字.call(对象,参数1,参数2,...);

```javascript
   function f1(x, y) {
       console.log("结果是:" + (x + y) + this);
       // return "10000";
   }
   // apply和call都可以让函数或者方法来调用,传入参数和函数自己调用的写法不一样,但是效果是一样的

   var result1 = f1.apply(null, [10, 20]);
   var result2 = f1.call(null, 10, 20);
   console.log(result1);  //30 object window
   console.log(result2);  //30 object window
```

apply和call方法中如果没有传入参数,或者是传入的是null,那么调用该方法的函数对象中的**this就是默认的window**
两个方法的区别在于apply需要传入的是一个数组，而call不是。

```javascript
   function f1(x, y) {
       console.log("这个函数是window对象的一个方法:" + (x + y) + this.sex);
   }
   window.f1(10, 20);    // 30 undefined
   //obj是一个对象
   var obj = {
       age: 10,
       sex: "男"
   };

   window.f1.apply(obj, [10, 20]);  //  30 男
   window.f1.call(obj, 10, 20);    //同上
```

```javascript
    function Person(age, sex) {
        this.age = age;
        this.sex = sex;
    }
    //通过原型添加方法
    Person.prototype.sayHi = function(x, y) {
        console.log("您好啊:" + this.sex);
        return x+y;
    };
    var per = new Person(10, "男");
    per.sayHi();

    console.log("==============");

    function Student(name, sex) {
        this.name = name;
        this.sex = sex;
    }
    var stu = new Student("小明", "男");
    var r1 = per.sayHi.apply(stu, [10, 20]);
    var r2 = per.sayHi.call(stu, 40, 20);

    console.log(r1);  //男  30
    console.log(r2);  //男   60
```

这两个方法的作用是：改变了this 的指向，并且第一个参数为你要传入的对象，传入后函数的this就指向了这个对象,后面的参数为你为函数传递的参数值。上面的两个数字就是sayHi方法中的两个参数。

## bind方法

bind，就是复制一份的意思。
使用的语法是：

* 函数名字.bind(对象,参数1,参数2,...);---->返回值是复制之后的这个函数
* 方法名字.bind(对象,参数1,参数2,...);---->返回值是复制之后的这个方法

```javascript
    function Person(age) {
        this.age = age;
    }
    Person.prototype.play = function() {
        console.log(this + "====>" + this.age);
    };

    function Student(age) {
        this.age = age;
    }
    var per = new Person(10);
    var stu = new Student(20);
    //将play方法复制了一份，然后传给stu，但是里面传进的参数还是使用的是stu的
    var ff = per.play.bind(stu);
    ff();    //输出object 20
```





