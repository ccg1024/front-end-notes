### Window

> window端记录，接linux端。

### 正则表达式后续

利用表达式过滤敏感词(**替换**)，获取特定的部分(**提取**)等。

```js
// normal pasword match, more than 6 char, less then 12 char
/^[a-z0-9_-]{6,12}.test('some_password')

// meta char: like '[a-z]'
```

元字符提高了灵活性与匹配能力

- 边界符：开头与结尾必须是什么字符
- 量词：重复的次数
- 字符类（'\d',表示0-9）

**量词**

除了.,*,?,+。还有下面的几个更常用的。

- {n}: repeat n times
- {n,}: repeat n or more times
- {n,m}: repeat between n and m times. **do not have space after ','**

> 在中括号中的'^'表示取反，即除了什么，例如除了小写字母'[^a-z]'

**字符类**

| 预定类 | 说明 |
| --- | --- |
| \d | like [0-9] |
| \D | like [^0-9] |
| \w | like [A-Za-z0-9_] |
| \W | like [^A-Za-z0-9_] |
| \s | match any space(space, tab...etc) |
| \S | match any not space char |

#### 修饰符

- i: ignore, 忽略大小写
- g: global, 全局匹配(类似vim中的替换操作的g)

#### 通过正则替换

配合字符串函数实现

```js
str.replace(/RegExp/, '')

// 自己写的下划线与驼峰命名法转换
function toUpperCase(str) {
  const newStr = str.replace(/.+?(_.)+/g, ($1, $2) => {
    console.log('first: ', $1)  // _name_y, xj_c
    console.log('seconde: ', $2)  // _y, _c
    // the return value will replace $1 value in str
    return $1.replace($2, $2[1].toUpperCase())
  })
  console.log(newStr)
}
console.log('_name_yxj_com_')
toUpperCase('_name_yxj_com_')
// out: _nameYxjCom_
```

### 用户登录

登录成功后，用户名等数据存储在`localStorage`中。

* * * *

### Iterators and generators

迭代器与生成器能够提供一种机制，让用户来自定义`for-of`循环。

#### 循环相关

处理熟知的循环方式，JS还提供标签语法来特别标识一个循环，通过标签来引用或配合break，continue来控制循环。

```js
// label statement
label:
  statement

// example
markLoop: while (theCondition) {
  doSomething()
}

// break markLoop
// will stop the while loop above.
```

#### for-in

for-in循环会迭代对象中的所有**可枚举**属性。但最好别用来循环数组，因为除了返回数组下标，还会返回用户自定义的属性——因为数组也是对象，只要是对象，就可以手动添加属性。

> 数组就是特殊的对象，系统自定义了下标属性，只需添加值。

#### for-of

for-of循环会创建一个循环来迭代**可迭代**对象。循环迭代的是**值**。而不是**属性**。但for-of迭代数组时，只会返回数组的值，用户自定义的属性值不会返回。

> 注意，是**可迭代对象**的值，一般自定义的Object不是可迭代对象，是不能够使用该循环的，但for-in就可以。这两者有明显区别。

* * * *

上面两个循环都可以配合解构赋值与`Object.entries()`来同时遍历对象的键值对。

```js
const obj = { foo: 1, bar: 2 };

for (const [key, val] of Object.entries(obj)) {
  console.log(key, val);
}
// "foo" 1
// "bar" 2
```
当`for-of`循环迭代一个可迭代对象过程。

1. 首先会调用对象的`[@@iterator]()`方法，该方法会返回一个迭代器（iterator）。

2. 通过重复调用该迭代器的`next()`方法来获取分配给变量的值序列。

3. 当`next()`方法返回的对象中包含`done:true`时，迭代完成。结束循环。

当`for-of`提前结束时，会调用迭代器的`return()`方法来执行一些清理操作。

**对象中的`[@@iterator]()`方法是放在`@@iterator`属性上的，该属性名称可以通过`Symbol.iterator`来获得，而不是手写**。

**同时，`[@@iterator]()`是无参数函数，且是被迭代对象自己调用的，也就是该函数中的`this`是指向迭代对象的**。该函数可以是普通函数，也可以是一个生成器函数，只要返回一个迭代器就可以。若是生成器函数，每个属性入口可以通过`yield`来提供。

当一个对象按照迭代器协议实现了`next()`方法时，该对象就是一个迭代器。所有的迭代器协议方法（`next`，`return`，`throw`）都返回`IteratorResult`接口的实现，即必须包含`done`与`value`属性。

`function*`声明一个生成器函数，该函数会返回一个生成器对象。同样生成器对象也可以通过构造函数`GeneratorFunction`生成。

```js
function* generator(i) {
  yield i
  yield i + 10
}
const gen = generator(10)
console.log(gen.next().value) // 10
console.log(gen.next().value) // 20
```

生成器同时满足**可迭代协议**与**迭代器协议**。`yield`用于暂停或恢复生成器函数。相当于顺序调用`next()`函数时的入口。

**迭代器**

通常，期望的是可迭代，不能迭代的迭代器是没什么意义的。官方定义为：一个对象，定义了一个序列，并可能在终止时返回一个值。

最常见的迭代器就是内置的数组迭代器，会返回数组中的序列，但不是所有迭代器都能够解释成数组，因为数组需要全额分配，但迭代器只有使用的时候才被消耗——即调用`next`。因此，迭代器可以被解释成一个不限大小的序列。

**案例**

```js
function makeRangeIterator(start = 0, end = Infinity, step = 1) {
  let nextIndex = start;
  let iterationCount = 0;

  const rangeIterator = {
    next() {
      let result;
      if (nextIndex < end) {
        result = { value: nextIndex, done: false };
        nextIndex += step;
        iterationCount++;
        return result;
      }
      return { value: iterationCount, done: true };
    },
  };
  return rangeIterator;
}

const it = makeRangeIterator(1, 10, 2);

let result = it.next();
while (!result.done) {
  console.log(result.value); // 1 3 5 7 9
  result = it.next();
}

console.log("Iterated over sequence of size:", result.value);
// [5 numbers returned, that took interval in between: 0 to 10]
```

**生成器**

虽然可以方便的自定义迭代器，但需要手动维护内部状态——上述案例中`rangeIterator`函数的状态。而生成器函数可以通过编写一个非连续执行的单一函数来自动维护。通过`function*`来定义。

生成器函数被调用时不会立马执行完所有代码，而是返回一个特殊类型的迭代器——生成器。当通过`next`消耗一个生成器值时，会继续运行代码，直到遇到`yield`关键字暂停。同样的生成器一次性消耗，生成器函数每次创建一个独立的生成器。

**将上述案例调整**

```js
function* makeRangeIterator(start = 0, end = Infinity, step = 1) {
  let iterationCount = 0;
  for (let i = start; i < end; i += step) {
    iterationCount++;
    yield i;
  }
  return iterationCount;
}

```

显然，使用生成器函数就可以避免编写大量的维护代码。

* * * *

**可迭代对象（Iterable）**

上述说的迭代器，生成器。最终是为了这里服务，**当对象定义了迭代行为**就可以认为对象可迭代。通常Object类型不是可迭代的。

为了实现可迭代，对象中或者原型链中必须实现`@@iterator`方法。也就是必须函数一个属性名`Symbol.iterator`。

可迭代对象可以多次迭代或一次迭代（例如生成器）。当`@@iterator`返回`this`时，只能迭代一次。若每次调用都返回新的迭代器，可以迭代多次。

```js
function* makeIterator() {
  yield 1
  yield 2
}
const it = makeIterator()

console.log(it[Symobl.iterator] === it) // true

// could itrate multi-time
it[Symbol.iterator] = function* () {
  yield 2
  yield 1
}
```

上述案例中，生成器是一个可迭代对象，但只能够迭代一次，因为`@@iterator`方法返回自身，若将其指向一个生成器函数，则可以迭代多次。

> ！！！使用自己维护迭代器案例中，迭代器自身不是可迭代对象，没有定义`@@iterator`方法。但生成器是可迭代对象，因为生成器函数帮忙维护了所有内容。

自定义迭代方法的方式为：

```js
const myIterable = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  },
};
```
### 用于可迭代对象的语法

- `for-of`

- `yield*`：用于代理另一个可迭代对象——将后面的可迭代对象返回。

```js
// yield*
function* gen() {
  yield* [1, 2, 3]
}
gen().next() // {value: 1, done: false}

function* fun1() {
  yield 42
}
function* fun2() {
  yield* fun1()
}
fun2().next() // {value: 42, done: true}
```

* * * *

**生成器优势**

1. `yield`让数据在需要时才进场计算。即高效，又节省开销

2. `next()`方法可接受一个参数，被`yield`捕获。可以用来修改内部状态，但第一次调用`next()`时，参数都被忽略。

3. `throw()`方法抛出一个异常，该异常会被挂起时的`yield`捕获，如没有，则向上传递异常，在下次调用`next()`时导致`done: true`。

4. `return(value)`方法将给定的值作为最终值返回，并结束迭代。

```js
function* gen() {
  let current = 0
  while(true) {
    let reset = yield current
    if (reset) {
      current = 0
    }
    current ++
  }
}
const it = gen()
it.next() // 0
it.next() // 1
it.next(true) // 0
it.next() // 1
```

* * * *

### Event Loop

事件循环是让JS实现异步的一种方式——本身是线程编程语言。通过事件循环来执行、收集和处理事件以及执行队列中的子任务。

一个JS运行时包含一个**待处理消息**的消息队列，每个消息都关联一个处理该消息的回调函数。在事件循环期间，消息队列中的消息会被作为与之关联的回调函数的参数传递过去。

当函数栈中的调用全部处理完全时，才会从消息队列中取出下一个消息进行处理。**这样的好处是，处理函数在运行时不会被抢占资源，但当一个消息的处理函数耗时太长时，浏览器将无法与用户进行其他互动，此时最好将一个消息分成多个**。

浏览器中，每一个有绑定事件处理行为的事件发生时，就会产生一个消息并添加到队列中。同样的对于`setTimeout`函数，该函数实际上接受的是一个待加入消息，与将消息添加到队列的最小延时。通常该函数会等消息队列中的消息都处理完后才加入队列。

```js
(function() {
  console.log('这是开始');
  setTimeout(function cb() {
    console.log('这是来自第一个回调的消息');
  });
  console.log('这是一条消息');
  setTimeout(function cb1() {
    console.log('这是来自第二个回调的消息');
  }, 0);
  console.log('这是结束');
})();

// "这是开始"
// "这是一条消息"
// "这是结束"
// "这是来自第一个回调的消息"
// "这是来自第二个回调的消息"
```

### 可枚举与所有权

通常，对象的默认属性都是可枚举的——即内部的可枚举标志为真，但通过`Object.defineProperty`定义的属性是不可枚举的。

对象的属性可以从三个方面进行分类：

- 枚举或不可枚举

- 字符类型或`Symbol`类型

- 自有属性或继承属性
