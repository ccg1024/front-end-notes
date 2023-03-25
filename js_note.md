javascript运行时间

- node.js runtime: backend vesion, also v8 engine
- browser runtime: v8 engine (chrome)

ECMAJavascript(es6(2015), es5)只是一种标准范式

Js基础类型(值类型)：boolean, string, number, bigint, undefined, symbol, null

引用数据类型(对象类型)：object(类似dict), array(类似list), function

特殊类型：regexp, date

```js
const objectVar = { prop1: 10, prop2: 20 }

console.log(objectVar.prop1)
console.log(objectVar['prop1'])
```

定义变量类型有：const, var(not recommend), let

> var问题：重定义问题（redeclare)

```js
// declaration
let variable;

// assignment
variable = 10;

// const keyword must show declaration and assignment
const variable2 = 10;
```

> 驼峰命名

```js
// some function use way
// immediately invoke
const var1 = (function () {
  return "run function";
})();
console.log(var1);  // will print: run function
```

```js
// arithmetic operators
8**2  // means 8^2

// comparison operators
20 === 20 // is 20 equal 20 in both value and type?
20 == '20' // will auto convert data type

// it's same as !=, !==
// 数值类型以外的变量无法通过基础运算符比较
```

```js
// arrow function, added in es6
const arrowFunction = () => {
  console.log('i am a arrow function')
}
// 修复了传统匿名函数中this绑定错误的问题

var obj = {
    birth: 1990,
    getAge: function () {
        var b = this.birth; // 1990
        var fn = function () {
            return new Date().getFullYear() - this.birth; // this指向window或undefined
        };
        return fn();
    }
};
```

> MDW WEB: a document web site for front-end

### object

**Js Date**

```js
const myDate = new Date();
myDate.valueOf();  // 原始值，从1970年1月1日开始经过的毫秒数
myDate.toString();  // 完整的时间内容
```

**Regular expression**

```js
// 创建正则表达式的三种方式
const re1 = /^.+@.+\..+$/; // /^.+@.+\..+$/i; 添加大小写忽略的修饰符
const re2 = new RegExp("^.+@.+\..+$");  // new RegExp("^.+@.+\..+$", "i");
const re3 = new RegExp(/^.+@.+\..+$/);  // new RegExp(/^.+@.+\..+$/, "i");

const userInput = "invalidemail@g";
re1.test(userInput);  // will return false

// 在正则中，*是0个与多个，+是至少一个。?0个或1个
// 第一种方法是最常用的
// example
/^.+@.+\..+$/.test(userInput);

// match group
/[A-Z]/.test(userInput)  // false; match any upper case
/[a-z]/.test(userInput)  // true; match any lower case
/[0-9]/.test(userInput)  // false; match any numbers
/(code|strik)/.test(userInput)  // false; matech code or strik in string
  
/[a-z]{5}/.exec(userInput)  // inval; 返回匹配到的5个字符，没数字默认匹配一个停止
```

**Array**

> 有种栈的感觉，方法push(), pop()。
> 方法slice()是一种浅拷贝(shallow copy)，返回对象与拷贝源的属性是一样的，都是指向相同的底层值。
> 这与深拷贝(deep copy)不同，深拷贝的两者是完全独立的。在浅拷贝中，对共享属性修改时，
> 两者都会发生改变，但对共享属性赋予新值，则不会。
> [example](https://developer.mozilla.org/en-US/docs/Glossary/Shallow_copy)

常用方法：

- push(), pop(), shift(), unshift()
- slice()
- splice()
- **map()**: 利用回调函数的返回值创建新的数组
- findIndex()
- forEach(): 只遍历，没有返回值
- includes()
- **filter()**: 返回通过回调函数的浅拷贝
- *reduce()*: 可以看做数组元素求和

**callback fucntion**

回调函数的运行情况如文件`sample.js`中展示的，通常，js自带的函数都需要配合回调函数来使用。

**lodash**

常用的Js库

**Math**

一个内置对象，所有方法都是静态的。不是一个构造器。

**error**

常见三种错误

- referenceError
- syntaxError
- typeError

```js
try {
  
} catch (e) {
  console.log(e.message);
}
```

**special type**

- NaN:
- null:
- undefined：为分配的变量初始值，没有返回值的函数的返回值

null, undefined在if条件中都被视为false，不会报错

```js
if (null) {
  console.log('this will not be reached');
} else {
  console.log('this will be reached');
}

if (undefined) {
  console.log('this will not be reached');
} else {
  console.log('this will be reached');
}
```

### 闭包(closure)

Js闭包能够让开发者可以从内部函数访问外部函数的作用域。闭包会随着函数的创建而被同时创建。

```js
function init() {
  let name = "Mozilla"; // name 是一个被 init 创建的局部变量
  function displayName() { // displayName() 是内部函数，一个闭包
      console.log(name); // 使用了父函数中声明的变量
  }
  displayName();
}
init();
```

闭包是由**函数与该函数的词法环境组合**而成的。该环境包含了**闭包创建时作用域内的任何局部变量**。可以使用闭包模拟私有方法。（数据隐藏与封装）

上面的案例中，`displayName()`函数就是一个闭包，处理自己独立的作用域空间，还共享外层函数的作用域空间。因此会同时保存两个作用域空间的变量。直到该闭包没有被任何人引用。

函数可以被多层嵌套。例如，函数 A 可以包含函数 B，函数 B 可以再包含函数 C。B 和 C 都形成了闭包，所以 B 可以访问 A，C 可以访问 B 和 A。因此，闭包可以包含多个作用域；他们递归式的包含了所有包含它的函数作用域。这个称之为作用**域链(C能够顺着C-B-A的顺序看到外面的变量)**。

> 感觉叫闭包的原因就是A看不到B中的东西，但B可以看到A中的东西。

### 定时器-间歇函数

每一秒钟自动运行一次的函数，例如网页倒计时等。

```js
let id = setInterval(callback, timeStep);

// timeStep 为毫秒
// 返回定时器的序号，类型为number。

//关闭定时器
clearInterval(id);
```

### 变量提升

- var：会被提升到函数或者文件顶部，并赋予初始值undefined
- let/const: 同样会被提升，但不会赋予任何初始值。如在声明前引用，就会referenceError。(这是开发网站上英文描述，但B站网课是说没有变量提升)

> 正常函数只有函数的声明被提升到顶部，函数表达式不会。

### 事件

事件监听三要素

- 事件源
- 事件类型
- 事件处理程序

### 原型（Prototype）

对象的隐藏属性，指向一个对象或null，与继承相关。

```js
let animal = {
  eat: 'eat foot'
}

let rabbit = {
  jump: 'jump jump',
  __proto__: animal  // 古早的方法，现在避免使用
}

// 如今常用，但一般不手动修改，浏览器处理慢
Object.setPrototypeOf(rabbit, animal);

console.log(rabit.eat)  // eat foot
```

#### 对象

处理直接用大括号创建对象，可以使用构造函数加new的方法创建对象。

```js
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

var mycar = new Car("Eagle", "Talon TSi", 1993);
```

所谓原型，就是java，python这样的语言中的父类，基类等。原型继承可以一直往下传递，但不能形成闭环。传递的链就是原型链。

#### 绑定this的运行函数的方法

**apply()**

在制定this的情况下运行函数。接受两个参数，一个为this的指向，一个为传递给函数的参数数组。

```js
let thisObj = null;
Math.max(thisObj, [1, 2, 3, 4])  // max函数运行时，this指向null。
```

**call()**

效果同apply，不同的是call接受参数列表。

```js
let thisObj = null;
Math.max(thisObj, 1, 2, 3, 4, 6)  // return 6
```

**bind()**

函数调用bind时，返回绑定this后的函数版本，可以在bind中预置绑定函数调用时传递的参数。

```js
const fun1 = Math.max.bind(null, 1, 2, 3, 4);
fun1()  // 4

const fun2 = Math.max.bind(null)
fun2(1, 2, 3, 4)  // 4
```

#### getters/setters

类似java语言的设计思想，写法上与一般的方法不同。

```js
var o = {
  a: 7,
  normalMethod: () => {},
  get b() {
    return this.a + 1;
  },
  set c(x) {
    this.a = x / 2
  }
};

console.log(o.a); // 7
console.log(o.b); // 8
o.c = 50;
console.log(o.a); // 25

// 为已经创建的对象添加 getter与setter

var o = { a:0 }

Object.defineProperties(o, {
    "b": { get: function () { return this.a + 1; } },
    "c": { set: function (x) { this.a = x / 2; } }
});

o.c = 10 // Runs the setter, which assigns 10 / 2 (5) to the 'a' property
console.log(o.b) // Runs the getter, which yields a + 1 or 6

// 为已有的对象添加属性与getter/setter
var d = Date.prototype;
Object.defineProperty(d, "year", {
  get: function() { return this.getFullYear() },
  set: function(y) { this.setFullYear(y) }
});

var now = new Date();
console.log(now.year); // 2000
now.year = 2001; // 987617605170
console.log(now);
// Wed Apr 18 11:13:25 GMT-0700 (Pacific Daylight Time) 2001
```

### Class

javascript中的类只不过是原有的原型机制的抽象。

```js
class MyClass {
  // Constructor
  constructor() {
    // Constructor body
  }
  // Instance field
  myField = "foo";
  // Instance method
  myMethod() {
    // myMethod body
  }
  // Static field
  static myStaticField = "bar";
  // Static method
  static myStaticMethod() {
    // myStaticMethod body
  }
  // Static block
  static {
    // Static initialization code
  }
  // Fields, methods, static fields, and static methods all have
  // "private" forms
  #myPrivateField = "bar";
}
```
类不会被提升，类似于let于const。通过new创建类的实例时，其原型链指向如下。

```js
class Cat {}

const cat = new Cat();
// cat.prototype -> Cat.prototype -> Object
```

`new`关键字的操作

1. 创建一个空的简单 JavaScript 对象（即 {}）；

2. 为步骤 1 新创建的对象添加属性 __proto__，将该属性链接至构造函数的原型对象；

3. 将步骤 1 新创建的对象作为 this 的上下文；

4. 如果该函数没有返回对象，则返回 this。

**Accessor fields**

访问字段，作用类似类中的get与set方法。区别如下。

```js
// java like
class Example1 {
  #value;  // private field
  
  constructor(value) {
    this.#value = value;
  }
  
  getValue() {
    return this.#value;
  }

  setValue(value) {
    this.#value = value;
  }
}

const example1 = new Example1(1);
console.log(example1.getValue())  // 1

// accessor field
class Example2 {
  constructor(value) {
    this.val = value;
  }
  get value() {
    return this.val;
  }
  set value(value) {
    this.val = value;
  }
}

const example2 = new Example2(1)
console.log(exampl2.value)  // 1
// 看似第二个类有一个value属性，实际上只有两个方法。
```

Js中，一个对象的直接原型只有一个，同时，派生类只有一个父类。

### Promise

Promise 是一个对象，它代表了一个异步操作的最终完成或者失败。本质上 Promise 是一个函数返回的对象，我们可以在它上面绑定回调函数，这样我们就不需要在一开始把回调函数作为参数传入这个函数了。

```js
// nomal way
createAudioFileAsync(audioSettings, successCallback, failureCallback);

// with promise
const promise = createAudioFileAsync(audioSettings);
promse.then(successCallback, failureCallback);
```

> 让异步能够预期的顺序执行。then之后的代码会在前面的执行完后再执行。
> async/await 两个关键字就是新增的能够实现Promise功能。

### 迭代器(iterator)与生成器(generator)

在 JavaScript 中，迭代器是一个对象，它定义一个序列，并在终止时可能返回一个返回值。更具体地说，迭代器是通过使用 next() 方法实现 Iterator protocol 的任何一个对象，该方法返回具有两个属性的对象： value，这是序列中的 next 值；和 done ，如果已经迭代到序列中的最后一个值，则它为 true 。如果 value 和 done 一起存在，则它是迭代器的返回值。

生成器函数提供了一个强大的选择：它允许你定义一个包含自有迭代算法的函数，同时它可以自动维护自己的状态。生成器函数使用 function*语法编写。最初调用时，生成器函数不执行任何代码，而是返回一种称为 Generator 的迭代器。通过调用生成器的下一个方法消耗值时，Generator 函数将执行，直到遇到 yield 关键字。

> 生成器可以生成迭代器，生成器返回的结果能够赋值给对象的[Symbol.iterator]，从而使该对象具有迭代器的接口。

```js
var myIterable = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
    // or yield* [1, 2, 3]
  }
}

for (let value of myIterable) {
    console.log(value);  // 1, 2, 3
}
```

用于可迭代对象的语法：for-of，展开语法，yield*，解构赋值。

```js
for (let value of ['a', 'b', 'c']) {
    console.log(value);
}
// "a"
// "b"
// "c"

[...'abc']; // ["a", "b", "c"]

function* gen() {
  yield* ['a', 'b', 'c'];
}

gen().next(); // { value: "a", done: false }

[a, b, c] = new Set(['a', 'b', 'c']);
a; // "a"
```

### 事件流

两个阶段，事件捕获与事件冒泡：事件捕获从顶端往下走(dom->element)，事件冒泡从底端往上走(element->dom)。通常的监听事件默认是事件冒泡形式。因此多个监听事件触发时，先处理最底层元素的。若需要设置监听事件在捕获阶段触发。则需要添加第三个参数。

```js
document.addEventListener('click', ()=> {}, true)  // 在捕获阶段触发
document.addEventListener('click', (event)=> {event.stopPropagation()}, true)  // 阻止冒泡与捕获
```

通过添加监听事件的情况下，回调函数若为匿名函数，无法解绑。因为解绑需要传递相同的函数。

```js
button.onClick = fn;
button.onClick = null;

button.addEventListener('click', fn);
button.removeEventListener('click', fn);
```

#### 事件委托

多个子元素需要触发相同类型的事件时，可以将触发事件委托给父元素，能够有效减少对子类的循环绑定监听事件。原理是通过事件冒泡来实现。

```js
ul.addEventListener('click', () => {});
// click li element of ul, the callback function will be invoked.
```

### 时间戳

通常应用于倒计时设置。1970年1月1日到当前的毫秒数，就是时间戳。所以，倒计时的设置就是用未来的时间戳减去当前的时间戳。

```js
// time stamp
// according time stamp to get date.

const current = +new Date();
const future = +new Date('2023-4-1 08:00:00'):

const timeStamp = (future - current) / 1000;  // convert to seconde.

// days remaining
let d = parseInt(timeStamp / 60 / 60 / 24)

// hours remaining
let h = parseInt(timeStamp / 60 / 60 % 24);

// minutes remaining
let m = parseInt(timeStamp / 60 % 60);

// seconds remaining
let s = parseInt(timeStamp % 60);

```
