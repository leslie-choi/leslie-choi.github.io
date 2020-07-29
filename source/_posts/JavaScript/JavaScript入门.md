---
title: JavaScript入门
date: 2019-01-26 16:16:24
tags: basic
categories: JavaScript
---

# JavaScript入门（一）

## js中的数据类型

js中的原始数据类型:number , string , boolean , null , undefined , object

number:数字类型(整数和小数)

string:字符串类型(的值一般都是用单引号或者是双引号括起来)  "34"

boolean:布尔类型(值只有两个,true(真1),false(假0))
null:空类型,值只有一个:null,一个对象指向为空了,此时可以赋值为null
undefined:未定义,值只有一个:undefined
什么情况下的结果是undefined：变量声明了,没有赋值,结果是undefined
函数没有明确返回值,如果接收了,结果也是undefined
如果一个变量的结果是undefined和一个数字进行计算,结果:NaN不是一个数字,也没有意义

如果想要获取一个变量的数据类型，可以使用typeof获取

```javascript
    var num = 10;
    var str = "小白";
    var flag = true;
    var nll = null;
    var undef;
    var obj = new Object();
    //是使用typeof 获取变量的类型
    console.log(typeof num);//number
    console.log(typeof str);//string
    console.log(typeof flag);//boolean
    console.log(String(nll));//是null
    console.log(typeof nll);//不是null
    console.log(typeof undef);//undefined
    console.log(typeof obj);//object
    console.log(typeof(num));
```

## 类型转换

其他类型转数字类型有三种方式：

```javascript
//parseInt();//转整数

    console.log(parseInt("10"));//10
    console.log(parseInt("10afrswfdsf"));//10
    console.log(parseInt("g10"));//NaN
    console.log(parseInt("1fds0"));//1
    console.log(parseInt("10.98"));//10
    console.log(parseInt("10.98fdsfd"));//10

//2.parseFloat()//转小数

    console.log(parseFloat("10"));//10
    console.log(parseFloat("10afrswfdsf"));//10
    console.log(parseFloat("g10"));//NaN
    console.log(parseFloat("1fds0"));//1    
	  console.log(parseFloat("10.98"));//10.98
    console.log(parseFloat("10.98fdsfd"));//10.98

//3.Number();//转数字
   console.log(Number("10"));//10
   console.log(Number("10afrswfdsf"));//NaN
   console.log(Number("g10"));//NaN    
   console.log(Number("1fds0"));//NaN
   console.log(Number("10.98"));//10.98
   console.log(Number("10.98fdsfd"));//NaN
//总结:想要转整数用parseInt(),想要转小数用parseFloat()
//想要转数字:Number();要比上面的两种方式严格
```

## 其他类型转字符串类型

```javascript
//1    .toString()
    var num=10;
    console.log(num.toString());//字符串类型
//2  String();
    var num1=20;
    console.log(String(num1));
//如果变量有意义调用.toString()使用转换
//如果变量没有意义使用String()转换
```

## 其他类型转Boolean

```javascript
    //1  Boolean(值);
   console.log(Boolean(1));//true
   console.log(Boolean(0));//false
   console.log(Boolean(11));//true
```

## 操作符

算数运算符:  +  -  * / %
  算数运算表达式:由算数运算符连接起来的表达式
  一元运算符: 这个操作符只需要一个操作数就可以运算的符号  ++  --
  二元运算符: 这个操作符需要两个操作数就可以运算,
  三元运算符: a > b ? a : b
  复合运算符: +=  -=  *= /= %=
  复合运算表达式:由复合运算符连接起来的表达式
  var num=10;
  num+=10;------>就是:num=num+10;
  console.log(num);20

关系运算符: >  <  >=  <=  ==不严格的 ===严格的 !=不严格的不等 !==严格的不等
  关系运算表达式:由关系运算符连接起来的表达式
  关系运算表达式的结果是布尔类型
  逻辑运算符:
  &&---逻辑与--并且
  ||---逻辑或---或者
  !---逻辑非---取反--取非
  逻辑运算表达式:由逻辑运算符连接起来的表达式
  表达式1&&表达式2
  如果有一个为false,整个的结果就是false
  表达式1||表达式2
  如果有一个为true,整个的结果为true
  !表达式1
  表达式1的结果是true,整个结果为false
  表达式1的结果是false,整个结果为true

  赋值运算符:  =

# JavaScript入门（二）

## 流程控制的三种方式

1. 顺序结构:从上到下,从左到右执行的顺序,就叫做顺序结构（但是不严谨）

2. 分支结构:if语句,if-else语句,if-else if-else if...语句,switch-case语句,三元表达式语句

3. 循环结构:while循环,do-while循环,for循环,for-in循环

while循环特点:先判断,后循环,有可能一次循环体都不执行

do-while循环特点:先循环,后判断,至少执行一次循环体

## break关键字

如果在循环中使用,遇到了break,则立刻跳出当前所在的循环

## continue关键字

在循环中如果遇到continue关键字,直接开始下一次循环

## 函数

函数:把一坨重复的代码封装,在需要的时候直接调用即可
函数的作用:代码的重用
函数的定义：

语法:

```javascript
function 函数名字(){

​	函数体-----一坨重复的代码

}
```

函数的调用:

函数名();

函数的命名要遵循驼峰命名法

JS中创建函数的三种方法

1. 函数声明

```javascript
function add(a,b){
    return a + b
};
```

2. 函数表达式，又叫函数字面量

```javascript
var add = function (a,b){
    return a + b
};
```

**两者的区别：**解析器会先读取函数声明，并使其在执行任何代码之前可以访问；而函数表达式则必须等到解析器执行到它所在的代码行才会真正被解释执行。

3. 自执行函数（严格意义上也是属于函数表达式）

```javascript
(function (a,b){
    return a + b
}(2,3)
```

自执行函数相当于一个沙箱，它主要用于创建一个新的作用域，在此作用域内声明的变量，不会和其它作用域内的变量冲突或混淆，大多是以匿名函数方式存在，且立即自动执行。

## 数组

1. 通过构造函数创建数组

```javascript
var 数组名=new Array();

var array=new Array();//定义了一个数组
```

数组的名字如果直接输出,那么直接就可以把数组中的数据显示出来,如果没有数据,就看不到数据

2. 通过字面量的方式创建数组

```javascript
var 数组名=[];//空数组

var array=[10,2,3,4];
```

## arguments伪数组

1. 什么是伪数组?

* 伪数组是一个对象
* 伪数组具有Length属性，值必须为number 类型，如果这个对象的length不为0，那么必须要有按照下标存储的数据
* 伪数组没有数组的pop、push等方法

2. 如何判断是否为伪数组

* 是否为对象
* 是否具有Length属性
* Length属性的值是否存在，并且必须是number 类型

3. 伪数组转化为真数组

* Array.prototype.slice.call()
* [].prototype.slice.call()
* Array.from()
* 扩展运算符[...()]

4. arguments

arguments就是一个经典的伪数组。他是所有（非箭头）函数中可用的局部变量

对arguments使用typeof返回‘object’

* arguments.callee指向当前执行的函数
* arguments.callee.caller指向当前函数的上一级函数
* arguments.callee.caller.arguments 父函数的实参
* arguments.length指向传递给当前函数的参数数量 (返回函数实参的个数)

```javascript
fn(2,4);
fn(2,4,6);
fn(2,4,6,8);

function fn(a,b,c) {
    console.log(arguments);
    console.log(fn.length);         //获取形参的个数
    console.log(arguments.length);  //获取实参的个数
    console.log("----------------");
}
//输出结果
  Arguments(2) [2, 4, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  3
  2
  ----------------
  Arguments(3) [2, 4, 6, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  3
  3
  ----------------
  Arguments(4) [2, 4, 6, 8, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  3
  4
  ----------------
```

# JavaScript入门（三）

## 一、创建对象的三种方式

1. 调用系统的构造函数创建对象

var 变量名= new Object(); Object 是系统的构造函数  Array

```javascript
    //工厂模式创建对象
    function createObject(name,age) {
      var obj = new Object();//创建对象
      //添加属性
      obj.name = name;
      obj.age = age;
      //添加方法
      obj.sayHi = function () {
        console.log("阿涅哈斯诶呦,我叫:" + this.name + "我今年:" + this.age);
      };
      return obj;
    }
    //创建人的对象
    var per1 = createObject("小芳",20);
    per1.sayHi();
    //创建一个人的对象
    var per2 = createObject("小红",30);
    per2.sayHi();
```

2. 自定义构造函数创建对象

```javascript
    //自定义构造函数创建对象,我要自己定义一个构造函数,自定义构造函数,创建对象
    //函数和构造函数的区别；名字是不是大写(首字母是大写)
    function Person(name,age) {
      this.name=name;
      this.age=age;
      this.sayHi=function () {
        console.log("我叫:"+this.name+",年龄是:"+this.age);
      };
    }

    //自定义构造函数创建对象:先自定义一个构造函数,创建对象
    var obj=new Person("小明",10);
    console.log(obj.name);
    console.log(obj.age);
    obj.sayHi();
```

3. 字面量的方式创建对象

```javascript
    var obj={};
    obj.name="小白";
    obj.age=10;
    obj.sayHi=function () {
      console.log("我是:"+this.name);
    };
    obj.sayHi();
    var obj2={
      name:"小明",
      age:20,
      sayHi:function () {
        console.log("我是:"+this.name);
      },
      eat:function () {
        console.log("吃了");
      }
    };
    obj2.sayHi();
    obj2.eat();
```

总结：工厂模式和自定义构造函数的异同

共同点:都是函数,都可以创建对象,都可以传入参数

工厂模式:

* 函数名是小写

* 有new,

* 有返回值

* new之后的对象是当前的对象

* 直接调用函数就可以创建对象


自定义构造函数:

* 函数名是大写(首字母)

* 没有new

* 没有返回值

* this是当前的对象

* 通过new的方式来创建对象


## 二、json数据格式

JSON格式的数据:一般都是成对的,是键值对,

json也是一个对象,数据都是成对的,一般json格式的数据无论是键还是值都是用双引号括起来的

```javascript
    var json = {
      "name": "小明",
      "age": "10",
      "sex": "男"
    };
    //遍历对象,是不能通过for循环遍历,无序

    //key是一个变量,这个变量中存储的是该对象的所有的属性的名字
    for (var key in json) {
      console.log(key + "===========" + json[key]);
    }
```

## 三、简单数据类型和复杂数据类型

原始数据类型: number,string,boolean,undefined, null,object
基本类型(简单类型),值类型: number,string,boolean
复杂类型(引用类型):object
空类型:undefined,null

值类型的值在哪一块空间中存储? 栈中存储
引用类型的值在哪一块空间中存储?对象在堆上存储,地址在栈上存储

```javascript
var num=10;//值类型,值在栈上
var obj={};//复杂类型,对象在堆,地址(引用)在栈
```

值类型之间传递,传递的是值
引用类型之间传递,传递的是地址(引用)

值类型作为函数的参数,传递的是值
引用类型作为函数的参数,传递的是

## 四、内置对象

# JavaScript入门（四）

## BOM内容

BOM:Browser Object Model:浏览器对象模型

BOM中的顶级对象就是window

DOM中页面中有一个顶级对象:Document

window----皇上。页面中所有的内容都是window,变量是属于window的,函数也是属于window,对象都是window的（但是实际开发过程为了方便和维护，可以将window省略）
document--总管太监

## BOM中的三个对话框

```javascript

alert("哈哈,我又变帅了");
window.alert("哦也");
window.prompt("请输入内容");

```
PS：alert对话框是浏览器自带的，所以无法修改其样式。建议使用其他ui自带的对话框。比如element-ui的dialog

## 页面的加载事件

```javascript
    //页面的加载事件,页面中所有的内容加载完毕后才执行，这个onload 其实可以自定义名称，比如myonload
   window.onload=function () {
     //通过id获取元素
     var btnObj=document.getElementById("btn");
     console.log(btnObj.value);
   };
```

## 定时器

```javascript
//js中设置定时器的第一种方法
	//参数1:函数
    //参数2:时间---1000毫秒---1秒
    //返回值:该定时器的id
    //执行过程:页面加载后,过了1秒才执行函数中的代码,函数中的代码执行完毕后,又过了一秒再执行函数中的代码,只要定时器不清理,就是执行方式
   var timeId = setInterval(function () {
     alert("你瞅啥?瞅你咋地?");
   },1000);
    //清理定时器的方法
    clearInterval(timeId);
//js中设置定时器的第二种方法
//定时器 异步运行
    function hello(){
    alert("hello");
    }
    //使用方法名字执行方法
    var t1 = window.setTimeout(hello,1000);
    var t2 = window.setTimeout("hello()",3000);//使用字符串执行方法
    window.clearTimeout(t1);//去掉定时器
```

两种方法根据不同的场景和业务需求择而取之，

一般情况下setTimeout用于延迟执行某方法或功能，是一次性的

setInterval则一般用于刷新表单，对于一些表单的假实时指定时间刷新同步

**setTimeout()使用注意点：**

setTimeout函数调用写法有:

1. setTimeout(fn, timeout)

2. setTimeout('fn()', timeout)

```javascript

//没加引号就直接执行函数showTime了……造成递归死循环，导致内存的溢出

     function show(){
      console.log('233')
      setTimeout('show()',1000)
     }
     show()

```

## JavaScript的location对象

```javascript
    //地址栏上#及后面的内容
    console.log(window.location.hash);
    //主机名字和端口号
    console.log(window.location.host);
    //主机名字
    console.log(window.location.hostname);
    //文件的相对路径
    console.log(window.location.pathname);
    //端口
    console.log(window.location.port);
    //协议
    console.log(window.location.protocol);
    //搜索的内容:获取的是?及后面的内容
    console.log(window.location.search);
    //和location.href属性是一样的操作,
    location.assign("http://www.baidu.com");
    //替换的地址的,没有历史记录
    location.replace("http://www.baidu.com");
    location.reload();//刷新
```

## javaScript的history对象

```javascript
<input type="button" value="跳一下" id="btn1"/>
第一个页面
<input type="button" value="去吧" id="btn2"/>
<script>
  document.getElementById("btn1").onclick=function () {
      location.href="11test.html";
  };
  document.getElementById("btn2").onclick=function () {
    //前进
    history.forward();
  };

  //想要前进和后退,必须要有历史记录
  history.forward();//前进
  history.back();//后退
  history.go();//如果是正数就是前进,负数就是后退
```

## DOM内容

## 变量提升和函数提升

变量提升:

```javascript
console.log(a)  //undefined
var a = "233"

//以上例子相当于
var a
console.log(a) 
a = "233"
```

变量提升会导致一个问题就是变量覆盖：

```javascript
var a;
console.log(a); // undefined
a = "233";
var foo = () => {
    var a; // 全局变量会被局部作用域中的同名变量覆盖
    console.log(a); // undefined
    a = "a1";
}
foo();
```

函数提升：

```javascript
  console.log(foo1); // [Function: foo1]

  foo1() // foo1

  function foo1 () {
    console.log("foo1")
  }


  console.log(foo2) // undefined
  
  foo2() // TypeError: foo2 is not a function

  var foo2 = function () {
    console.log("foo2")
  }
```

```javascript
var getName = function(){
  console.log(2)
}

function getName(){
  console.log(1)
}

getName()   //  由于函数声明的提升，所以输出2

//相当于转化为下面的代码
var getName

function getName(){
  console.log(1)
}

getName = function(){
  console.log(2)
}

getName()   //输出2
```

即函数提升只会提升函数声明，而不会提升函数表达式。

## for-in 循环、 for-of 循环和foreach

### 遍历数组

```javascript
  let arr = [1,2,3,4,5]
  arr.forEach(item => console.log(item))  //1,2,3,4,5

  for(let i in arr){
    console.log(i)        //  输出的是key,即对应的索引0,1,2,3,4
  }

  for(let [i] of arr){
    console.log(i)        //  输出的是value,即对应的值1,2,3,4,5
  }

  for(let [k,v] of arr.entries()){
    console.log(k,v)
  }
//  0 1
//  1 2
//  2 3
//  3 4
```

### 遍历字符串

```javascript
  let str = 'hello'

//str.forEach(item => console.log(item))    //  报错，foreach是用于遍历数组的api

  for(let i in str){
    console.log(i)        //  输出的是key,即对应的索引0,1,2,3,4
  }

  for(let i of str){
    console.log(i)        //  输出的是value,即对应的值h,e,l,l,o
  }
```

### 遍历对象

```javascript
     let obj = {
        name : "james",
        age: 15
     }
    //1. 使用for in遍历
     for(let item in obj){
      console.log(`${item} : ${obj[item]}`)     //name : james  age : 15
     }

    //2. 使用Object.keys()和Object.values()
     for(let item of Object.keys(obj)){         //  也可以使用Object.values()取值
      console.log(item)                       //name    age
     }

    // 3.使用getOwnPropertyNames
    Object.getOwnPropertyNames(obj).forEach(function(key){
        console.log(key+ '---'+obj[key])
    })
```

### 遍历Map 和Set

遍历Set；

```javascript
     let arr = new Set([1,2,3,4,5])

     for(let item in arr){
      console.log(item)     //  无输出，因为Set结构没有键名，只有键值
     }

     for(let item of arr){
      console.log(item)   //  1，2，3，4，5
     }
```

遍历Map：

```javascript
    let map = new Map([
        ['name' ,"james"],
        ['age' ,28],
      ])

    for(let [key,value] of map){
      console.log(key,value)       //  name james   age 28
    }

    for(let item in map){
      console.log(item)       //  name age
    }

    for(let item in map){
      console.log(map[item])    // james  28
    }
```

1. foreach 方法没办法使用 break 语句跳出循环，或者使用return从函数体内返回

2. for...of

* 可以避免所有 for-in 循环的陷阱
* 不同于 forEach()，可以使用 break, continue 和 return
* for-of 循环不仅仅支持数组的遍历。同样适用于很多类似数组的对象
* 它也支持字符串的遍历
* for-of 并不适用于处理原有的原生对象

能够被for...of正常遍历的，都需要实现一个遍历器Iterator。
而数组、字符串、Set、Map结构，早就内置好了Iterator（迭代器），它们的原型中都有一个Symbol.iterator方法，而Object对象并没有实现这个接口，使得它无法被for...of遍历。

## 四则运算

* 运算中其中⼀⽅为字符串，那么就会把另⼀⽅也转换为字符串 
* 如果⼀⽅不是字符串或者数字，那么会将它转换为数字或者字符串

```javascript
1 + '1' // '11' 
true + true // 2 
4 + [1,2,3] // "41,2,3"
'a' + + 'b' // -> "aNaN" 
```

对于第⼀⾏代码来说，触发特点⼀，所以将数字 1 转换为字符串，得到结果 '11' 
对于第⼆⾏代码来说，触发特点⼆，所以将 true 转为数字 1 
对于第三⾏代码来说，触发特点⼆，所以将数组通过 toString 转为字符串 1,2,3 ，得 到结果 41,2,3
对于第四行代码来说，因为 + 'b' 等于 NaN ，所以结果为 "aNaN" ，你可能也会在⼀些代码中看到过 + '1' 的形式来快速获取 number 类型。 

```javascript
4 * '3' // 12 
4 * [] // 0 
4 * [1, 2] // NaN
```

**那么对于除了加法的运算符来说，只要其中⼀⽅是数字，那么另⼀⽅就会被转为数字**

## == 和 === 有什么区别

**对于 == 来说，如果对⽐双⽅的类型不⼀样的话，就会进⾏类型转换**

假如我们需要对⽐ x 和 y 是否相同，就会进⾏如下判断流程

1. ⾸先会判断两者类型是否相同。相同的话就是⽐⼤⼩了 
2. 类型不相同的话，那么就会进⾏类型转换 
3. 会先判断是否在对⽐ null 和 undefined ，是的话就会返回 true 
4. 判断两者类型是否为 string 和 number ，是的话就会将字符串转换为 number

```javascript
1 == '1'      //转换为
1 ==  1 
```

5. 判断其中⼀⽅是否为 boolean ，是的话就会把 boolean 转为 number 再进⾏判断

```javascript
'1' == true 
'1' ==  1 
 1  ==  1 
```

**对于 === 来说就简单多了，就是判断两者类型和值是否相同**

Object.is相对于 === 的改进，就是

```javascript
Object.is(-0,+0)  //  false
Object.is(NaN,NaN)  //  true
```

## 深浅拷⻉

**浅拷⻉**

1. ⾸先可以通过 Object.assign 来解决这个问题，很多⼈认为这个函数是⽤来 深拷⻉的。其实并不是， Object.assign 只会拷⻉所有的属性值到新的对象中，如果属性值是对象的话，拷⻉的是地址，所以并不是深拷⻉。但是要注意的是：**当object只有一层的时候，是深拷贝。**

```javascript
let a = {   
  age: 1 
} 
let b = Object.assign({}, a) 
a.age = 2 
console.log(b) // age: 1
console.log(a)  // age: 2
```

2. 另外我们还可以通过展开运算符 ... 来实现浅拷⻉

```javascript
let a = {   
  age: 1,   
  jobs: {     
    first: 'FE'   
  } 
} 
let b = { ...a } 
a.jobs.first = 'native' 
console.log(b.jobs.first) // native
```

浅拷⻉只解决了第⼀层的问题，如果接下去的值中还有对象的话，那么就⼜回到Y开始的话题了，两者享有相同的地址。要解决这个问题，我们就得使⽤深拷⻉了。

深拷⻉

这个问题通常可以通过 JSON.parse(JSON.stringify(object)) 来解决。

```javascript
let a = {   
  age: 1,   
  jobs: {     
    first: 'FE'   
  } 
}

let b = JSON.parse(JSON.stringify(a)) 
a.jobs.first = 'native' 
console.log(b.jobs.first) // FE
```

但是该⽅法也是有局限性的：

* 会忽略 undefined 
* 会忽略 symbol 
* 不能序列化函数 
* 不能解决循环引⽤