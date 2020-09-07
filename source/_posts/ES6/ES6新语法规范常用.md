---
title: ES6新特性
date: 2019-04-13 23:06:21
tags: Interview
categories: JavaScript
---

# 关键字**let**和**const**

## let关键字的使用

let声明的变量只在let命令所在的**代码块内有效，是块级作用域的前提，不允许重复声明，不存在变量提升，存在暂时性死区**（加粗的是let和const的相同点）。
PS：暂时性死区的本质是，只要进入当前作用域，所要使用的变量就已经存在但是不可获取，只有等到变量声明的那一行代码出现，才可以获取和操作该变量。

const声明一个只读的常量，一旦声明，常量的值就不能改变，所以声明的时候必须初始化，同时也不存在变量提升。
let的基本用法：

```JavaScript
{
  let a = 0;
  var b = 1;
  console.log(a);   // 0
}
console.log(a);    // 报错 ReferenceError: a is not defined
console.log(b);//  1 
//=========>分割线
//let只能声明一次，而var可以声明多次，所以for的循环计时器适合用let
let a = 1;
let a = 2;
var b = 3;
var b = 4;
console.log(a); // Identifier 'a' has already been declared
console.log(b);  // 4
```

举一个栗子：

```JavaScript
for (var i = 0; i < 10; i++) {
  setTimeout(function(){
    console.log(i);
  })
}
// 输出十个 10
for (let j = 0; j < 10; j++) {
  setTimeout(function(){
    console.log(j);
  })
}
//输出0到9
```

var声明的变量在全局范围内只有一个变量i，循环里的十个 setTimeout 是在循环结束后才执行，用于计数的变量泄露成为全局变量，所以此时的 i 都是 10；
变量 j 是用 let 声明的，当前的 j 只在本轮循环中有效，每次循环的 j 其实都是一个新的变量，所以 setTimeout 定时器里面的 j 其实是不同的变量，即最后输出0123456789。（JS引擎内部会记住前一次循环的值），所以这与JS的运行机制并不冲突。

注意点：使用let声明变量。并不存**在变量提升**。

```JavaScript
console.log(a);  //ReferenceError: a is not defined
let a = "apple";
 
console.log(b);  //undefined
var b = "banana";
```

## const关键字的使用

const 声明一个只读变量，声明之后不允许改变。意味着，一但声明**必须初始化**，否则会报错。但是如果是一个对象，可以改变对象里面属性值。

**const使用的注意点：**
**其实 const保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动**。但是简单类型和复合类型保存值的方式是不同的。对于简单类型（数值 number、字符串 string 、布尔值 boolean）,值就保存在变量指向的那个内存地址，因此 const 声明的简单类型变量等同于常量。而复杂类型（对象 object，数组 array，函数 function），变量指向的内存地址其实是保存了一个指向实际数据的指针，所以 const 只能**保证指针是固定**的，至于指针指向的数据结构则无法来控制了，所以使用 const 声明复杂类型对象时要慎重。

在 ES6 快速入门中，对于 let 和 const ，建议优先选择 const，尤其是在全局环境中，不应该设置变量，只应设置常量

* const 可以提醒阅读程序的人，这个变量不应该改变
* const 比较符合函数式编程思想，运算不改变值，只是新建值，而且这样也有利于将来的分布式运算
* JavaScript 的编译器会对const 进行优化，所以多使用 const 有利于提高程序的运行效率



## ES6 声明变量的六种方法

ES5 只有两种声明变量的方法：var命令和function命令。ES6 除了添加let和const命令，另外两种声明变量的方法：import命令和class命令。所以，ES6 一共有 6 种声明变量的方法。

# ES6 提供了对函数的扩展

## ES6为参数**提供了默认值**
在定义函数时便初始化了这个参数，以便在参数没有被传递进去时使用。

```javascript
function A(a,b=1){
    console.log(a+b);
}
A(1);    //2
A(2,3); //5
```

## 箭头函数

在es6中，提供了一种简洁的函数写法，我们称作“箭头函数”。

写法：函数名=(形参)=>{……}     当函数体中只有一个表达式时，{}和return可以省略，当函数体中形参只有一个时，()可以省略。所以当函数体内代码多于1行时，必须使用{}，并且使用return语句返回

在函数执行时，**this 总是指向调用该函数的对象**。要判断 this 的指向，其实就是判断 this 所在的函数属于谁

**特点：箭头函数使得this的指向固定化，this始终指向箭头函数定义时的离this最近的一个函数，如果没有最近的函数就指向window**

箭头函数的注意事项：

1. 函数体内的 this 对象就是定义时所在的对象，而不是使用是所在的对象
2. 不可以单作构造函数，所以就不可以使用 new 命令，否则会抛出错误
3. 不可以使用 arguments 对象，该对象在函数体内不存在。如果需要使用可以用 rest 参数代替
4. 不可以使用 yield 命令，因此箭头函数不能作 Generator 函数

```JavaScript
//省略写法
var f1 = f => { hhh }

//es5写法
var f1 = function(f){
    return hhh
}
```

# 对象的扩展 

## 属性的简写。

ES6 允许在对象之中，直接写变量。这时，属性名为变量名, 属性值为变量的值。

```JavaScript
var foo = 'bar';
var baz = {foo};  //等同于  var baz = {foo: foo};
```

## 方法的简写，省略冒号与function关键字

```JavaScript
var o = {
  method() {
    return "Hello!";
  }
};
 
// 等同于
var o = {
  method: function() {
    return "Hello!";
  }
};
```

## Object.keys()方法

获取对象的所有属性名或方法名（不包括原形的内容,for...in循环遍历出原型对象的属性，可以使用hasOwnProperty()判断），返回一个数组。

```JavaScript
var obj={name: "john", age: "21", getName: function () { alert(this.name)}};
console.log(Object.keys(obj));    // ["name", "age", "getName"]
console.log(Object.keys(obj).length);    //3

console.log(Object.keys(["aa", "bb", "cc"]));    //["0", "1", "2"]
console.log(Object.keys("abcdef"));    //["0", "1", "2", "3", "4", "5"]
```

## Object.values()方法

获取对象的所有属性值，返回一个数组。

```JavaScript
var obj={name: "john", age: "21", getName: function () { alert(this.name)}};
console.log(Object.values(obj))    // ["john", "21", ƒ]
console.log(Object.keys(obj).length)    //  3
```

## Object.entries()

将对象的属性名和属性值作为一个数组的两个元素，插入到另一个空数组中。

```javascript
const person = {
  name: 'james',
  age: 18,
  sex: 'man'
}
const man = Object.entries(person)
console.log(man)  // [['name','james'],['age',18],['sex','man']]
```

## for...of  循环

是遍历所有数据结构的统一的方法。for...of循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如arguments对象、DOM NodeList 对象）、Generator 对象，以及字符串。(因为对象没有Iterator接口，所以无法被遍历)

```javascript
var arr=["小林","小吴","小佳"];
for(var v of arr){
    console.log(v);	
}
//小林 
//小吴 
//小佳
```

PS:其他循环

### for...in 循环

```javascript
  const myArray = [1,2,3]
  for (var index in myArray) { // 实际代码中不要这么做，使用for...of循环更合适
    console.log(myArray[index]);
  }
```

* 数组的键名是数字，但是for...in循环是以字符串作为键名“0”、“1”、“2”等等。如果你使用字符串的 index 去参与某些运算（"2" + 1 == "21"），运算结果可能会不符合预期。

* for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。(比如继承关系的时候，就有些麻烦了，遍历的时候会把继承的属性也遍历出来)

* 某些情况下，for...in循环会以任意顺序遍历键名。

* 总之，for...in循环主要是为遍历对象而设计的，不适用于遍历数组。

PS:如何解决for...in循环遍历出原型对象的属性。使用hasOwnProperty()判断

```javascript
class Animal{
  constructor(name,age){
    this.name = name
    this.age = age
  }
}

const dog = new Animal()
Animal.prototype.height = 99
dog.weight = 18
for(let item in dog){
  if(dog.hasOwnProperty(item)){
    console.log(item)
  }
}
//name
//age
//weight,而height是原型对象上的属性，所以不会输出
```

### forEach循环

```javascript

myArray.forEach(function (item,index,arr) {
  console.log(item,index,arr)
})

```

这种写法的问题在于，无法中途跳出forEach循环，break命令也不能用return语句从闭包函数中返回，所以没有返回值

# 模块化的支持

ES6标准中，JavaScript原生支持模块(module)了。这种将JS代码分割成不同功能的小块进行模块化，将不同功能的代码分别写在不同文件中，各模块只需导出公共接口部分，然后通过模块的导入的方式可以在其他地方使用。

export用于对外输出本模块（一个文件可以理解为一个模块）变量的接口。

import用于在一个模块中加载另一个含有export接口的模块。

import和export命令只能在模块的顶部，不能在代码块之中。

```JavaScript
//导入部分
//全部导入
import Person from './example'
 
//将整个模块所有导出内容当做单一对象，用as起别名
import * as example from "./example.js"
console.log(example.name)
console.log(example.getName())
 
//导入部分
import { name } from './example'
 
//导出部分
// 导出默认
export default App
 
// 部分导出
export class User extend Component {};
```

## 五、模板字符串

所有的空格和缩进都会被保留在输出之中。。

```javascript
`Hello ${name}`

//如果说不想要换行，就可以使用trim方法去除空格和换行

$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`.trim())
```

# 新增 symbol 基础数据类型

JS中的数据类型分为基本数据类型还有复杂数据类型(引用数据类型)

复杂数据类型：object、function、array、date、基本包装类、内置对象（存放在堆中）
基本数据类型：字符串、数字、布尔值、null和undefined。ES6引入了第6种——Symbol（存放栈中）

PS：两种类型的区别是：存储位置不同

原始数据类型直接存储在栈(stack)中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；

引用数据类型存储在堆(heap)中的对象,占据空间大、大小不固定,如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体

![markdown](https://leslie-blog.oss-cn-hongkong.aliyuncs.com/leslie_choi_blog/dizhi.png)

Symbol功能类似于一种标识唯一性的ID

ES5的对象属性名都是字符串，很容易造成属性名冲突。比如，使用了一个他人提供的对象，想为这个对象添加新的方法，新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的，这样就从根本上防止了属性名冲突。这就是ES6引入Symbol的原因，防止某一个键被不小心改写或覆盖

通常情况下，我们可以通过调用Symbol()函数来创建一个Symbol实例。

Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 值不是对象，所以不能添加属性。基本上，它是一种类似于字符串的数据类型。

```JavaScript

let s1 = Symbol()
typeof Symbol()   //symbol
let firstName = Symbol("first name")    //Symbol函数接受一个可选参数，可以添加一段文本来描述即将创建的Symbol，这段描述不可用于属性访问，但是建议在每次创建Symbol时都添加这样一段描述，以便于阅读代码和调试Symbol程序

Object.defineProperties(person, {
    [firstName]: {
        value: "match",
        writable: false
    }
})                                // 让该属性变为只读
```

## 应用场景

1. 使用Symbol来替代常量

```JavaScript

const TYPE_AUDIO = 'AUDIO'
const TYPE_VIDEO = 'VIDEO'
const TYPE_IMAGE = 'IMAGE'

```

使用Symbol，就不必一直为起变量的名字头疼了。一下这样定义就可以保证三个变量是完全独一无二的

```JavaScript

const TYPE_AUDIO = Symbol()
const TYPE_VIDEO = Symbol()
const TYPE_IMAGE = Symbol()

```

2. 使用Symbol定义类的私有属性/方法

JavaScript中，是没有如Java等面向对象语言的访问控制关键字private的，类上所有定义的属性或方法都是可公开访问的。因此这对我们进行API的设计时造成了一些困扰。
有了Symbol以及模块化机制，类的私有属性和方法才变成可能。
新建一个a.js文件。

```JavaScript

const PASSWORD = Symbol()
class Login {
  constructor(username, password) {
    this.username = username
    this[PASSWORD] = password
  }
  checkPassword(pwd) {
      return this[PASSWORD] === pwd
  }
}
export default Login

```
另建b.js

```JavaScript

import Login from './a'

const login = new Login('admin', '123456')

login.checkPassword('123456')  // true
login.PASSWORD  // 无法访问到
login[PASSWORD] // 同上
login["PASSWORD"] // 同上

```
由于Symbol常量PASSWORD被定义在a.js所在的模块中，外面的模块获取不到这个Symbol，也不可能再创建一个一模一样的Symbol出来（因为Symbol是唯一的），**因此这个PASSWORD的Symbol只能被限制在a.js内部使用**，所以使用它来定义的类属性是没有办法被模块外访问到的，达到了一个私有化的效果。

3. 注册和获取全局Symbol

通常情况下，我们在一个浏览器窗口中（window），使用Symbol()函数来定义和Symbol实例就足够了。但是，如果你的应用涉及到多个window（最典型的就是页面中使用了 iframe ），并需要这些window中使用的某些Symbol是同一个，那就不能使用Symbol()函数了，因为用它在不同window中创建的Symbol实例总是唯一的，而我们需要的是在所有这些window环境下保持一个共享的Symbol。这种情况下，我们就需要使用另一个API来创建或获取Symbol，那就是**Symbol.for()**，它可以注册或获取一个window间全局的Symbol实例：

```JavaScript

let gs1 = Symbol.for('global_symbol_1')  //注册一个全局Symbol
let gs2 = Symbol.for('global_symbol_1')  //获取全局Symbol

gs1 === gs2  // true

```

这样一个Symbol不光在单个window中是唯一的，在多个相关window间也是唯一的了。

4. 属性检索

Symbol作为属性名，该属性不会出现在for...in、for...of循环中，也不会被Object.getOwnPropertyNames()、Object.keys()、JSON.stringify()返回。于是，在ES6中添加了一个Object.getOwnpropertySymbols()方法来检索对象中的Symbol属性

Object.getOwnPropertySymbols()方法的返回值是一个包含所有Symbol自有属性的数组

```JavaScript

let uid = Symbol.for("uid")
let object = {
    [uid]: "12345"
}

let symbols = Object.getOwnPropertySymbols(object)
console.log(symbols.length) // 1
console.log(symbols[0]) // "Symbol(uid)"
console.log(object[symbols[0]]) // "12345"

```

# 新增 set 数据类型

ES6中新增了Set数据结构，类似于数组，但是 它的成员都是唯一的 ，其构造函数可以接受一个数组作为参数。

```javascript
let array = [1, 1, 1, 1, 2, 3, 4, 4, 5, 3];
let set = new Set(array);
console.log(set); // 此时打印的是set 结构 Set(5) {1, 2, 3, 4, 5}
console.log(Array.from(set)); //将Set结构强制转换为数组
```

# 新增 Map 数据

其实数组也是集合, 只不过数组的索引是数值类型.当想用非数值类型作为索引时, 数组就无法满足需要了.

而 Map 集合可以保存多个键-值对(key-value), Set 集合可以保存多个元素.

对Map 和 Set 一般不会逐一遍历其中的元素. Map 一般用来存储需要频繁取用的数据, Set 一般用来判断某个值是否存在其中.
键和值都可以是任意类型。键的比较使用的是Object.is()，因此你可以将5与“5”同时作为键，因为他们类型不同。对象也可以作为 key . 这比用对象来模拟的方式就灵活了很多

* set(key, value): 向其中加入一个键值对
* get(key): 若不存在 key 则返回 undefined
* has(key):返回布尔值
* delete(key): 删除成功则返回 true, 若key不存在或者删除失败会返回 false
* clear(): 将全部元素清除

Set是无重复值的有序列表。Set会自动移除重复的值，因此你可以使用它来过滤数组中重复的值并返回结果。
Map是有序的键值对，其中的键允许是任何类型。
Set和Map是es6新增的两个数据集合。

map() 方法创建一个新数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

# class、extends、super

这三个特性设计到ES5几个点，就是原型，继承还有多态，确实是令人挺烦的。。但那是ES6之前的事了蛤蛤。

ES6提供了更接近传统语言的写法，引入了Class（类）这个概念。新的class写法让对象原型的写法更加清晰、更像面向对象编程的语法，也更加通俗易懂。

```javascript
class MyClass {
  constructor() {  // 构造函数
    // ...
  }
  get prop() {  // 取值
    return 'getter';
  }
  set prop(value) { // 存值
    console.log('setter: '+value);
  }
}

let inst = new MyClass();
inst.prop = 123;
// setter: 123
console.log(inst.prop);
// 'getter'
```

extends用法

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }
}

class ColorPoint extends Point {
  constructor(x, y, color) {
    this.color = color; // ReferenceError
    super(x, y);
    this.color = color; // 正确
  }
}
```
在子类的构造函数中，**只有调用super之后**，才可以使用this关键字，否则会报错。

其实 super(…) 做的事情就是绑定 this。因为在原型继承中，如果一个类要继承自另一个类，那就得先实例化一次它的父类作为作为子类的原型。如果不做这件事，子类的原型就不能确定，当然也就无法创建 this。所以如果在 constructor 中没有 super(…) 就企图获取 this 就会报错。

这是因为子类实例的构建，是基于对父类实例加工，只有super方法才能返回父类实例。父类的静态方法，也会被子类继承。

注意，super虽然代表了父类Point的构造函数，但是返回的是子类ColorPoint的实例，即super内部的this指的是ColorPoint，因此super()在这里相当于**Point.prototype.constructor.call(this)。**

作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。

```javascript
    class A {}
    class B extends A {
      m() {
         super(); // 报错
      }
    }
```

PS：static 关键字解释：类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用， 这就称为“ 静态方法”。

# 解构赋值

## 数组的解构赋值

```javascript
	const arr = [122,321,344]
	const [a,b,c] = arr
	console.log(a,b,c)    //122,321,344

  let [head,...tail] = [1,2,3,4]
  head  //1
  tail  //2,3,4

  let [x,y] = ["a"]
  y   // undefined，如果结构不成功，那么变量的值就是undefined
```
如果右边不是可以遍历的结构，那么将会报错。**事实上只要某种数据结构具有 iterator 接口，都可以使用数组形式的解构赋值**

```javascript
//报错
let [foo] = 1
let [foo] = false
let [foo] = NaN
let [foo] = undefined
let [foo] = null
let [foo] = {}
```

并且允许指定默认值:

```javascript
let [foo = true] = []
foo //true

let [x = 1] = [null]
x //null，如果一个数组成员是null，那么默认值就不会生效
```

## 对象的解构赋值

```javascript

	const obj = {name: "james",age: 66}
	const {name:myName,age} = obj
	console.log(myName,age)   //james 66

  //对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值是由它的位置决定的；
  //而对象的属性没有次序，变量必须与属性同名才能取到正确的值
```

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。如果解构失败，变量的值等于undefined。

```javascript
let { bar, foo } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: 'aaa', bar: 'bbb' };
baz // undefined
```

## 字符串的解构赋值

字符串的解构赋值，会使得字符串被转换成一个类似数组的对象

```javascript

const [a,b,c,d,e] = 'hello'
a //  "h"
b //  "e"
c //  "l"
d //  "d"
e //  "o"

```

## 解构赋值的用途

1. 交换变量的值

```javascript
  let x = 1
  let y = 2
  [x, y] = [y, x]    //交换变量x和y的值，这样的写法不仅简洁，而且易读，语义非常清晰。
```

2. 从函数返回多个值

函数只能返回一个值，如果要返回多个值，只能将它们放在数组或对象里返回。有了解构赋值，取出这些值就非常方便。

```javascript
// 返回一个数组
function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();

```
      
3. 提取JSON 数据

```javascript

let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
}

let { id, status, data: number } = jsonData;

console.log(id, status, number);
// 42, "OK", [867, 5309]

```

4. 遍历 Map 结构

任何部署了 Iterator 接口的对象，都可以用for...of循环遍历。Map 结构原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便。

```javascript

const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world

```

如果只想获取键名，或者只想获取键值，可以写成下面这样。

```javascript
// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}

```

# 尾调用和尾递归

## 尾调用（Tail Call）

是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。

```javascript
function f(x){
  return g(x);
}
//最后一步调用并不是指在函数的尾部，只要是最后异步操作即可：

function f(x) {
  if (x > 0) {
    return m(x)
  }
  return n(x);
}
// 上面代码中，函数m和n都属于尾调用，因为它们都是函数f的最后一步操作。
//值得注意的是，以下三种情况均不是尾调用：

// 情况一
function f(x){
  let y = g(x);
  return y;   //   尾调用之后还有操作
}

// 情况二
function f(x){
  return g(x) + 1;
}

// 情况三
function f(x){
  g(x);
}
```
上面代码中，情况一是调用函数g之后，还有赋值操作，所以不属于尾调用，即使语义完全一样。情况二也属于调用后还有操作，即使写在一行内。情况三等同于调用g(x)后return undefined。

## 尾调用优化

函数调用会在内存形成一个“调用记录”，又称“调用帧”（call frame），保存调用位置和内部变量等信息。如果在函数A的内部调用函数B，那么在A的调用帧上方，还会形成一个B的调用帧。等到B运行结束，将结果返回到A，B的调用帧才会消失。如果函数B内部还调用函数C，那就还有一个C的调用帧，以此类推。所有的调用帧，就形成一个“调用栈”（call stack）。
尾调用由于是函数的最后一步操作，所以不需要保留外层函数的调用帧，因为调用位置、内部变量等信息都不会再用到了，只要直接用内层函数的调用帧，取代外层函数的调用帧就可以了。

```javascript

function b(item) {
  return item
}
// 下面是尾调用例子
function a() {
  let m = 1
  let n = 2
  return b(m + n)
}
a()
// 上面例子实际上等同于:
b(3)
```
上面代码中，如果函数b不是尾调用，函数f就需要保存内部变量m和n的值、b的调用位置等信息。但由于调用b之后，函数a就结束了，所以执行到最后一步，完全可以删除a(x)的调用帧，只保留b(3)的调用帧。

这就叫做“尾调用优化”（Tail call optimization），即只保留内层函数(即b函数)的调用帧。如果所有函数都是尾调用，那么完全可以做到每次执行时，调用帧只有一项，这将大大节省内存。这就是“尾调用优化”的意义。

**注意，只有不再用到外层函数(函数f)的内部变量，内层函数(函数g)的调用帧才会取代外层函数的调用帧，否则就无法进行“尾调用优化”**

```javascript

function addOne(a){
  var one = 1;
  function inner(b){
    return b + one;
  }
  return inner(a);
}

```
显而易见，上面的函数不会进行尾调用优化，因为内层函数inner用到了外层函数addOne的内部变量one。

## 尾递归

递归非常耗费内存，因为需要同时保存成千上百个调用帧，很容易发生“栈溢出”错误（stack overflow）。但对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。

### 例子一：求阶乘

```javascript

function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}

factorial(5) // 120

```

让我们来理一下上面函数的顺序：
进入之后 由于 n !== 1 进入 return n * factorial(n - 1) 即 5 * factorial(4)
第二次进入 n === 4 , return 4 * factorial(3)
知道 n === 1, return 1出来相当于变成了 5 * 4 * 3 * 2 * 1 = 120
上面代码是一个阶乘函数，计算n的阶乘，最多需要保存n个调用记录，复杂度 O(n)(可以理解为调用帧个数) 。

```javascript

function factorial(n, total) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5, 1) // 120

function factorial(n, total = 1) {
  if (n === 1) return total;
  return factorial(n - 1, n * total);
}

factorial(5) // 120
//也可以直接给total一个默认值等于1，那么调用的时候只给n赋值就可以

```

这里传了两个参数，第二个参数为total,我们来看一下函数的运作：
第一次进入：直接调用 factorial(5-1, 5 * 1) 即 factorial(4, 5)
第二次进入：调用 factorial(4-1, 4 * 5) 即 factorial(3, 20)
第三次进入：调用 factorial(3-1, 3 * 20) 即 factorial(2, 60)
第四次进入：调用 factorial(2-1, 2 * 60) 即 factorial(1, 120)
第五次进入：因为n === 1, 所以 return 120

### 例子二:斐波那契数列

```javascript

  //不使用尾递归求斐波那契数列
		function Fibonacci(n){
			if(n < 0){
				return false
			}
			if(n <= 1){
				return 1
			}
			return Fibonacci(n-1) + Fibonacci(n-2)
		}
//上面的例子由于从上到下递归，会有非常多的重复的运算，导致消耗大量的性能

  //提高性能版本
	function Fibonacci(n){
		if(n <= 0){
			return 0
		}else if(n <= 2){
			return 1
		}else{
			//数列存放在数组中，第n项对应数组的第n-1个数
			let result = []
			result[1] = 1
			result[2] = 2
			for(var i = 3;i < n;i++){
				result[i] = result[i-1] + result[i-2]
			}
		  return result[n-1]//数列的第三项就是数组的第二项
		}
	}

	// 使用ES6的尾递归提高性能
	function Fibonacci(n, num1 = 1,num2 = 1) {
	 	if(n <= 1){return num2}
	 	return Fibonacci(n - 1,num2,num1 + num2)
	}
	Fibonacci(4)
```

值得注意的是：
**ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。**

这是因为在正常模式下，函数内部有两个变量，可以跟踪函数的调用栈。

* func.arguments：返回调用时函数的参数。
* func.caller：返回调用当前函数的那个函数。
* 尾调用优化发生时，函数的调用栈会改写，因此上面两个变量就会失真。严格模式禁用这两个变量，所以尾调用模式仅在严格模式下生效。

尾递归优化只在严格模式下生效，那么正常模式下，或者那些不支持该功能的环境中，有没有办法也使用尾递归优化呢？回答是可以的，就是自己实现尾递归优化。

它的原理非常简单。尾递归之所以需要优化，原因是调用栈太多，造成溢出，那么只要减少调用栈，就不会溢出。怎么做可以减少调用栈呢？就是采用“循环”换掉“递归”。


# 新增 Map 和 Set 数据结构

```javascript
 var map = new Map()
 var set = new Set()
 console.log(typeof(map))   //object
 console.log(typeof(set))    //object

```

## Set 数据结构

ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

Set 本身是一个构造函数，用来生成 Set 数据结构。

```javascript

const s = new Set();
[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));
  for (let i of s) {
    console.log(i);
  }   // 2 3 5 4

```

上面代码通过add方法向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值。

Set 函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

利用 Set 数据结构的特性，可以实现数组的去重。

```javascript

  var arr = [1,2,3,4,5,5,5]
  console.log(Array.from(new Set(arr)))
  console.log([...new Set(arr)])

```

**Set实例的属性和方法**

* Set.prototype.constructor：构造函数，默认就是Set函数。
* Set.prototype.size：返回Set实例的成员总数。

### Set 实例的操作方法（用于操作数据）

* add(value)：添加某个值，返回 Set 结构本身。
* delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
* has(value)：返回一个布尔值，表示该值是否为Set的成员。
* clear()：清除所有成员，没有返回值。

```javascript

  let s = new Set()
  s.add(1).add(2).add(2);
  // 注意2被加入了两次

  s.size // 2

  s.has(1) // true
  s.has(2) // true
  s.has(3) // false

  s.delete(2);
  s.has(2) // false

```

### Set 实例的遍历方法（用于遍历成员）

* keys()：返回键名的遍历器
* values()：返回键值的遍历器
* entries()：返回键值对的遍历器
* forEach()：使用回调函数遍历每个成员

```javascript

let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red green blue

for (let item of set.values()) {
  console.log(item);
}
// red green blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]

```

由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以keys方法和values方法的行为完全一致。这意味着，可以省略values方法，直接用for...of循环遍历 Set。

entries方法返回的遍历器，同时包括键名和键值，所以每次输出一个数组，它的两个成员完全相等。

foreach方法则和数组类似。

## Map 数据结构

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

为了解决这个问题，ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

```javascript

const m = new Map();
const o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false

```

上面代码使用 Map 结构的set方法，将对象o当作m的一个键，然后又使用get方法读取这个键，接着使用delete方法删除了这个键。

上面的例子展示了如何向 Map 添加成员。作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

```javascript

  const map = new Map([
    ['name', '张三'],
    ['title', 'Author']
  ]);

  map.size // 2
  map.has('name') // true
  map.get('name') // "张三"
  map.has('title') // true
  map.get('title') // "Author"

```

**注意，只有对同一个对象的引用，Map 结构才将其视为同一个键。这一点要非常小心！！**

```javascript
const map = new Map();

map.set(['a'], 555);
map.get(['a']) // undefined

```

上面代码的set和get方法，表面是针对同一个键，但实际上这是两个值，内存地址是不一样的，因此get方法无法读取该键，返回undefined。

同理，同样的值的两个实例，在 Map 结构中被视为两个键。

```javascript

  const map = new Map();

  const k1 = ['a'];
  const k2 = ['a'];

  map
  .set(k1, 111)
  .set(k2, 222);

  map.get(k1) // 111
  map.get(k2) // 222

```

上面代码中，变量k1和k2的值是一样的，但是它们在 Map 结构中被视为两个键。

由上可知，Map 的键实际上是跟内存地址绑定的，只要内存地址不一样，就视为两个键。这就解决了同名属性碰撞（clash）的问题，我们扩展别人的库的时候，如果使用对象作为键名，就不用担心自己的属性与原作者的属性同名。

如果 Map 的键是一个简单类型的值（数字、字符串、布尔值），**则只要两个值严格相等，Map 将其视为一个键**，比如0和-0就是一个键，布尔值true和字符串true则是两个不同的键。另外，undefined和null也是两个不同的键。虽然NaN不严格相等于自身，但 Map 将其视为同一个键。

```javascript

let map = new Map();

map.set(-0, 123);
map.get(+0) // 123

map.set(true, 1);
map.set('true', 2);
map.get(true) // 1

map.set(undefined, 3);
map.set(null, 4);
map.get(undefined) // 3

map.set(NaN, 123);
map.get(NaN) // 123

```

### Map 实例的属性和操作方法

1. size 属性

size属性返回 Map 结构的成员总数。

```javascript

const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2

```

2. set(key, value)

set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。

```javascript

const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined

//set方法返回的是当前的Map对象，因此可以采用链式写法。

let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c')

```

3. get(key)

get方法读取key对应的键值，如果找不到key，返回undefined。

```javascript

const m = new Map();

const hello = function() {console.log('hello');};
m.set(hello, 'Hello ES6!') // 键是函数

m.get(hello)  // Hello ES6!

```

4. has(key)

has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

```javascript

const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');
m.set(undefined, 'nah');

m.has('edition')     // true
m.has('years')       // false
m.has(262)           // true
m.has(undefined)     // true

```

5. delete(key)

delete方法删除某个键，返回true。如果删除失败，返回false。

```javascript

const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false

```
6. clear()

clear方法清除所有成员，没有返回值。

```javascript

let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0

```

### Map 实例的遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

* keys()：返回键名的遍历器。
* values()：返回键值的遍历器。
* entries()：返回所有成员的遍历器。
* forEach()：遍历 Map 的所有成员。

需要特别注意的是，Map 的遍历顺序就是插入顺序。

```javascript

  const map = new Map([
    ['F', 'no'],
    ['T',  'yes'],
  ]);

  for (let key of map){
    console.log(key)
  }
//    ['F', 'no'],
//    ['T',  'yes']

  for (let key of map.keys()) {
    console.log(key);
  }
  // "F"
  // "T"

  for (let value of map.values()) {
    console.log(value);
  }
  // "no"
  // "yes"

  for (let item of map.entries()) {
    console.log(item[0], item[1]);
  }
  // "F" "no"
  // "T" "yes"

  // 或者
  for (let [key, value] of map.entries()) {
    console.log(key, value);
  }
  // "F" "no"
  // "T" "yes"

  // 等同于使用map.entries()
  for (let [key, value] of map) {
    console.log(key, value);
  }
  // "F" "no"
  // "T" "yes"

```

### Map 和数组互相转换

Map转换为数组，使用扩展运算符(...)

```javascript
  const map = new Map([
    [1, 'one'],
    [2, 'two'],
    [3, 'three'],
  ]);

  [...map.keys()]
  // [1, 2, 3]

  [...map.values()]
  // ['one', 'two', 'three']

  [...map.entries()]
  // [[1,'one'], [2, 'two'], [3, 'three']]

  [...map]
  // [[1,'one'], [2, 'two'], [3, 'three']]
```

数组转换为Map 构造函数，将数组传入 Map 构造函数，就可以转为 Map。

```javascript

new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }

```

### Map 和对象的互相转换

Map 转为对象

如果所有 Map 的键都是字符串，它可以无损地转为对象。

```javascript

function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

对象转为 Map

```javascript

function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}

```

# babel 转码原理

babel的转译过程分为三个阶段：parsing(解析)、transforming(转换)、generating(产生)

具体过程：

1. 编写ES6代码
2. babylon进行解析，解析得到AST
3. plugin用babel-traverse对AST树进行遍历转译，得到新的AST树
4. 用babel-generator通过AST树生成ES5代码

需注意的一点就是，babel默认只是转译新标准引入的语法，比如ES6的箭头函数，不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。

# Promise

# Proxy

Proxy 是 ES6 中新增的功能，可以⽤来⾃定义对象中的操作。

Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

```javascript
let p = new Proxy(target, handler); // `target` 代表需要添加代理的对象 // `handler` ⽤来⾃定义对象中的操作 可以很⽅便的使⽤ Proxy 来实现⼀个数据绑定和监听 
 
let onWatch = (obj, setBind, getLogger) => { 
  let handler = {     
    get(target, property, receiver) {       
      getLogger(target, property)       
      return Reflect.get(target, property, receiver)  
      },     
    set(target, property, value, receiver) {       
      setBind(value);       
      return Reflect.set(target, property, value)
      }   
    }
  return new Proxy(obj, handler);
}

let obj = { a: 1 } 
let value 
let p = onWatch(obj, (v) => {   
  value = v }, 
  (target, property) => {   
  console.log(`Get '${property}' = ${target[property]}`); 
}) 

p.a = 2 // bind `value` to `2` p.a // -> Get 'a' = 2
```

# 其他

## Array.find 简写

```javascript
const pets = [{
    type: 'Dog',
    name: 'wangcai'
  },
  {
    type: 'Cat',
    name: 'tom'
  },
  {
    type: 'Dog',
    name: 'Tommy'
  }
]
pet = pets.find(item => item.type === 'Dog' && item.name === 'Tommy')

console.log(pet) // { type: 'Dog', name: 'Tommy' }
```

## 隐式返回

```javascript
const implicitReturn = (value) => (value + value) // 注意这里是圆括号 不是花括号
console.log(implicitReturn('giao'))
```

## charAt() 简写

```javascript
'SampleString'.charAt(0) // S
// 简写
'SampleString'[0]
```

## 有条件的函数的调用

```javascript
function fn1() {
  console.log('I am Function 1')
}

function fn2() {
  console.log('I am Function 2')
}
// 复杂写法
let checkValue = 3
if (checkValue === 3) {
  fn1()
} else {
  fn2()
}
// 简短写法
(checkValue === 3 ? fn1 : fn2)()
```

## Math.Floor 简写

```javascript
let val = '123.95'

console.log(Math.floor(val)) // 常规写法
console.log(~~val) // 简写
```

## Math.pow 简写

```javascript
Math.pow(2,3) // 8
2 ** 3 // 简写 8  
```

## 字符串转换为数字

```javascript
const num1 = parseInt('100')
// 简写
console.log(+"100")
console.log(+"100.2")
```

## && 运算

```javascript
let value = 1
if (value === 1)  console.log('Value is one')
// && 运算
value && console.log('Value is one')

//  只要“&&”前面是false，无论“&&”后面是true还是false，结果都将返“&&”前面的值

//  只要“&&”前面是true，无论“&&”后面是true还是false，结果都将返“&&”后面的值
```

## toString 简写

```javascript
let someNumber = 123
console.log(someNumber.toString()) // "123"

// 简写
console.log(`${someNumber}`) // "123"
```

## 使用对象的形式代替 switch 语法

```javascript
// switch 写法
let animal = 'dog'
let name
switch(animal) {
  case 'dog':
    name = 'wangcai'
    break
  case 'cat':
    name = 'tom'
    break
  case 'pig':
    name = 'paige'
    break
  default:
    name = 'jerry'
}

//  对象的形式，实际上是使用策略模式重构代码
// case1
const fn1 = (name) => {
  const animalsList = {
    'dog': () => { return 'wangcai' },
    'cat': () => { return 'tom' },
    'pig': () => { return 'peppa pig' },
    'default': () => { return 'jerry' }
  }
  if (typeof animalsList[name] !== 'function') {
      return false
  }
  return animalsList[name]()
}
fn1('dog')

// case2
const animalsList2 = {
  'dog': () => { return 'wangcai' },
  'cat': () => { return 'tom' },
  'pig': () => { return 'peppa' },
  'default': () => { return 'jerry' }
}

const fn2 = (name) => {
  return animalsList2[name]
}
fn2('cat')()
```

# 参考文章

(22+ 高频实用的 JavaScript 片段 （2020年）)[https://mp.weixin.qq.com/s/kmelc879Eadfr9YFntkAUg]