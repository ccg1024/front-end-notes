# Linux paltform

> 从archlinx中记录

通过document可以进行节点克隆。

```js
// element.cloneNode(boolean)
// true, 当前节点的子元素也会克隆
// false, 子元素不会克隆，默认值。
```

删除节点必须通过父元素进行删除。

```js
fatherNode.removeChild(sonNode)
```

M端事件（移动端事件），常见的三个事件为：

|event|description|
|----|----|
|touchstart|finger touch the element|
|touchmove|finger move on element|
|touchend|finger out the element|

> 通常M端事件都是通过插件来实现的，常用的插件为：swiper。尤其是轮播功能。
> 通常通过document进行操作都会有性能问题，所以在React框架中最好用ref。

### 延时函数

延迟一段时间执行回调函数，只会调用一次。`setTimeout(callback, millisecond)`，但最好还是在后面进行清除：`clearTimeout(id)`

### 同步/异步

js为单线程语言，通常，同步任务是放在**执行栈**中，异步任务是放在**任务队列**中，通过回调函数执行。

当执行栈中的所有任务都执行完毕，才会调用任务队列中的异步任务。

```js
console.log(1)
setTimeout(fn, 0)
console.log(2)

// 执行栈
// console.log(1)
// console.log(2)

// 任务队列
// setTimeout(fn, 0)
```

**事件循环(event loop)过程**

![Js执行机制](./img/Js执行机制.png)

通常异步任务是交给执行环境处理，一般是浏览器，或者node。

### location

> just means `window.location`

通常存储一些当前url相关的信息，因此可以通过修改对象上的url值实现页面跳转。

```js
// jump to baidu
location.href = 'https://www.baidu.com'
```

> 在url中添加#号，可以实现页面不跳转的spas应用（单页面网站）。例如https://www.baidu.com/#/no_jump

- reload(): 刷新页面，参数写入true，则强制刷新

### navigator

记录浏览器自身的信息，`userAgent`信息就存储在该对象中。

### history

记录浏览器地址栏的操作，实现类似前进后退的网页操作。

### 本地存储

实现页面刷新也不会导致数据丢失功能，将数据存储在浏览器中。容量较大。`sessionStorage`与`localStorage`约5M左右。

#### localStorage

作用：将数据永久存储在本地，除非手动删除。

特性：
- 多窗口（页面）共享（同一浏览器可以共享）
  > 不能够跨域，即百度网址存的东西在京东上的localStorage是看不到的。
- 以键值对的形式存储

```js
// set
localStorage.setItem(key, value)

// get
localStorage.getItem(key)

// remove
localStorage.removeItem(key)
```

#### sessionStorage

特性：
- 生命周期为关闭浏览器窗口
- 在同一各窗口（页面）下数据共享
- 以键值对形式存储
- 用法同localStorage相同

> 开发中少用`id`属性，能够用该属性的地方大部分应该都能够通过`ref`来代替

> **本地存储的所有数据都会被转化成字符串形式**

**存储复杂数据类型**

由于只能存储字符串，因此若直接存对象类型的数据将导致无法使用该数据——（在浏览器中会被同一转化成字符串`[Object Object]`）。

通常是将复杂数据转化成Json字符串形式进行本地存储。

```js
// JSON.stringify(object)
const obj = {pro: 1}
const jsonStr = JSON.stringify(obj)
localStorage('obj', jsonStr)

// conver json string to object
const jsonObj = JSON.parse(localStorage.getItem('obj'))
```