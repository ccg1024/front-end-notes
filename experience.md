---
title: 面经总结
desc: 牛客面试经验
date: 2023-4-11
---
# 面经

### js

- js 中变量类型

7 个基本数据类型：boolean, number, bigInt, string, undefined, null, Symbol

1 个引用数据类型：object

- promise 是什么以及使用方法：

将三种状态，promise 对象方法，微任务，会在宏任务后面立即执行。

- 什么是跨域，如何解决：

同源限制，协议，域名，端口，CORS，node 中间件，JSONP，postmessage。

node 中间件，nginx 反向代理，就是通过代理进行请求发送，前端与代理服务器之间是同源的，然而，服务器之间是不存在同源限制的。

JSONP 的原理是，跨域问题是请求文件产生的，但请求`script`是不会产生跨域问题。同时设置响应头文档类型为`javascript`。因为脚本标签本来就是能够引用网络上的 JS 文件的。使用的是 GET 请求。在不适用 JSONP 的时候，该标签是通过请求拿到脚本文件直接执行，若使用的是 JSONP，则返回的是客户端传过去的回调函数与文件内容（作为回调参数），然后在本地执行回调函数。

postmessage 则是 H5 的新端口，通过发送与接受 API 实现同信。

- JS 中判断变量类型

一共四种方法，typeof，instanceof，Object.prototype.toString.call(obj)对象原型链，constructor 引用数据类型，就是判断实例的构造函数与类的构造函数是否相同，避免了原型链干扰。最精准的就是第三种。

- JS 中实现异步的方式

回调函数，事件监听，setTimeout，Promise，生成器，async/awati。

> 回调函数——Ajax 回调。生成器 ES6 语法，因为该函数不是一次执行完，可以通过 yield 保存不同阶段状态，通过 next 返回。

- 数组去重方法

通过对象属性，就是遍历数组的时候，将数据作为零时对象的属性，若没有，则添加属性，若有，则抛弃数组值，Set 有兼容性问题，filter，reduce+includes 等，filter+indexOf，原理是后者只返回第一个匹配到的数据下标。

- undefined 与 null

undefined 是全局对象的一个属性，声明的变量为赋值，没有返回值的函数返回值。函数形参没有赋值，undefined==undefined，undefined===undefined，访问对象中的不存在属性。

null 代表**对象**的值未设置，相当没有设置指针地址。**null 的 typeof 是 object**。null 是人为设置的值。可以将对象赋值为 null 来释放对象。

- ES6 中的箭头函数

没有 this，从外部获取，没有 arguments，不能用 new，没有原型与 super。

通过`call`与`apply`调用时，第一个参数会被忽略，即无法绑定 this。无法使用`yield`关键字，因此箭头函数也不能够作为生成器函数。

- 说一说 HTML 语义化

语义化标签，利于页面内容结构化，利于无 CSS 页面可读，利于 SEO，利于代码可读。

1. 易于用户阅读，样式文件为加载时，页面结构清晰。

2. 易于 SEO，搜索引擎根据标签来确定上下文与关键字权重

3. 方便屏幕阅读器解析，例如盲人阅读器根据语义渲染网页

4. 利于开发和维护，代码可读性高

- this 指向问题

全局执行上下文、函数执行上下文、this 严格模式下 undefined、非严格模式 window、构造函数新对象本身、普通函数不继承 this、箭头函数无 this、可继承。

关键字由来，在对象内部的函数中使用对象内部的属性是非常普遍的需求，但 JS 作用域机制无法实现，就创造了 this 机制。

从全局执行上下文，函数执行上下文。

- JS 变量提升

注意函数提升是优先于变量，let，const 会有暂时性死区，初始化之前访问会报错。

- HashRouter 与 HistoryRouter 区别

> 二者都是通过浏览器的特性实现的。

1. history 是通过浏览器的历史记录栈 API 实现。hash 则是监听 location 对象的 hash 值变化事件实现。

2. url 差别。

3. 同一个 url 刷新，history 会触发浏览器历史记录栈，而 hash 不会。

4. history 需要后端配置指向 index.html，否则刷新会出现 404 问题。hash 不需要。

> 原理：HashRouter 通过`windown.onhashchange`方法获取新 URL 中的 hash 值。HistoryRouter 则是通过`history.pushState`来实现页面跳转是不触发页面刷新，通过`window.onpopstate`监听前进与后退。

- map 与 forEach 的差别

> 除了自己知道的，map 处理速度是比 forEach 快的，同时方便使用链式调用其他数组方法。

- **Event loop，宏任务，微任务**

得分点：任务挂起，同步任务执行结束，执行队列中异步任务，执行`script`标签内代码，`setTimeout/setInterval`，`ajax`，`postMessage/MessageChannel`，`setImmediate`，`I/O(nodejs)`，`Promise`，`MutationObserver`，`Object.oberve`,`process.nextTick(nodejs)`，每个宏任务中，都包含一个微任务队列。

---

**_浏览器的事件循环机制_**：执行 JS 代码时，遇见同步任务，就直接推入**调用栈**中执行，遇到异步任务，将该任务挂起，等到异步任务有返回之后，推入到队列之中，当**调用栈**中所有同步任务执行完毕，再将任务队列中的任务按顺序推入并执行。如此循环。

**_异步任务_**：异步任务又分为宏任务与微任务。

**宏任务**：任务队列中的任务，称为宏任务，**每个宏任务中都包含一个微任务队列**。

**微任务**：等宏任务中的主要功能都完成后，渲染引擎不着急去执行下一个宏任务，而是执行当前宏任务中的微任务。

宏任务包含：

1. 执行`script`标签内部代码

2. `setTimeout/setInterval`

3. `ajax`

4. `postMessage/MessegeChannel`

5. `setImmediate`

6. `I/O(nodejs)`

7. DOM 事件。

微任务包含:

1. `Promise`

2. `MutationObserver`（监视 dom 元素的改变）

3. `Object.observe`（监视某个对象属性的改变）

4. `process.nextTick(nodejs)`

> 浏览器与 node 环境中，微任务的执行时间不同，node 端中，微任务在事件循环的各个阶段之间执行，而浏览器中是在宏任务执行完后进行。

> 在分析事件循环过程中，**一整块代码就是一个宏任务**，因此，分析过程应该是：同步任务，当前文件中的微任务，文件中记录的宏任务。

---

### css

- 什么是 BFC

块级格式化上下文、独立渲染区域、不会影响边界以外的元素。产生 BFC 的条件：float，position，overflow，display。

使用 BFC 的目的是创建独立的渲染区域，内部的元素渲染不会影响外界。常见的应用场景是清除浮动（就是使用了`float`，属性是让元素沿着其容器的左侧或右侧放置，能够让文本与内联元素环绕它，会部分脱离文档流，也就是元素本身大小不会去撑开父容器。）。在前面的描述中，由于浮动元素脱离文档流，其父容器的渲染就会影响到外部的元素（因为浮动元素不会把父元素撑开）。为了让浮动元素撑开父容器——盒子内部的东西不会跑到外部。将父容器设置为 BFC 就可以了。但有因为浮动是部分脱离，所有，不会有兄弟元素跑到浮动元素的后背，被遮住。

产生方式

1. overflow 非 visible 赋值（最常用为 hidden）

2. float 设置左或右。

3. position 为绝对或固定。

4. display 设置为 flex，inline-block 等。

> 其他相关的内容，IFC（inline formatting context）内联格式化上下文，GFC（grid formatting context）网格布局格式化上下文，display 设置 grid，FFC（Flex formatting context）自适应格式化上下文，display 设置 flex 或 inline-flex。

- 伪类与伪元素：自己查找的

伪类：一种选择器，用于选择处于特定状态的元素，比如鼠标悬浮在某个元素上，某个类型中第一个元素等，就像对文档的某个部分应用了一个类一样。通常接在`:`后面。表现就像为特定部分添加了类。

伪元素：类似伪类，就像往文档中添加了新的 HTML 元素一样，而不是向现有元素上应用类。伪元素通常接在`::`后。例如一个`p`元素中有长文字，那么想让在屏幕上第一行变粗，其他不变，可以在文字上添加新标签，但当屏幕大小改变，则有需要重新调整新标签位置，而伪元素`::first-line`则会一直指向屏幕上第一行的内容，即便动态改变屏幕大小。

- 浮动：脱离文档流，盒子塌陷，影响其他元素排版，伪元素，`overflow:hidden`，标签插入法。

设置浮动元素，块级元素可以在同一行，行内元素可以设置宽高——就是自动转换成行内块。

通过伪元素清除浮动方式，与在父类添加 overflow 是一样的，`.clear-float::after{content:''; display: table; clear: both}`，前面是牛客上的写法，实际上，table 就是创建一个块级元素，clear 的意思是是否让元素移动到他之前的浮动元素下面，content 的内容就是伪元素显示的内容，这里为空。

- css 尺寸设计问题

px，rem，em，vw，vh。注意的是，em 用在其他属性上，是相对于自身字体大小。在移动端的响应式设计中，通过 rem 设置字体，然后，配合不同屏幕尺寸在根节点字体大小不同，展示的内容的字体大小也会相应改变。

- **元素水平垂直居中**

1. 父元素相对定位，目标元素绝对定位，设置 top，left 为 50%，同时移动自动大小的一半`transform: translate(-50%, -50%)`，兼容性最好

2. 父元素设置为 flex，同时设置`justify-content: center; align-items: center`，目前也是主流

3. 父元素设置为 grid，同时设置`justify-content: center; align-items: center`，同 flex

4. 父元素设置为`display: table-cell`，同时设置目标元素为`text-align: center; vertical-align: middle`

- 说一说三栏布局的实现方案

> 圣杯布局、双飞翼布局、三栏高度不定、中间栏内容多先渲染

三栏布局：要求左右两边的盒子宽度固定，中间盒子宽度自适应，盒子高度随着内容撑高。通常中间盒子内容较多，为保证页面渲染快，**在写结构的时候将中间盒子放在左右盒子之前**。实现三栏布局的常用方法是**圣杯布局**与**双飞翼布局**。

圣杯布局实现方案：三个元素放在同一个父元素中，代表中间盒子的元素放在最前，父元素设置左右`padding`，三个盒子全部浮动——左边与中间设置左浮动，右边设置右浮动，设置中间盒子宽度 100%，左右盒子固定宽度，**设置左边盒子`margin-left: -100%`，设置相对定义，左边(`left`)平移自身宽度**。右边盒子设置右边距为自身盒子宽度，最后设置父元素清除浮动，否则父元素无法被撑开。

双飞翼布局实现方案：三个盒子对应三个元素，其中中间盒子套两层，中间盒子内部盒子设置`margin`(用来移除左边盒子对中间盒子的覆盖)。三个盒子全浮动，设置中间盒子宽度 100%，左右盒子固定宽度。左边盒子左边距-100%，右边盒子设置右边距-自身宽度，最后设置父元素清除浮动。(父元素右边需要有 padding，不然右边盒子会不见)

> 加分项：圣杯布局优点：无需添加额外的 dom 节点。缺点：当中间部分的宽度小于左边盒子时，会出现布局混乱。双飞翼布局优点：不会出现布局混乱。缺点：多添加一层 dom 节点。

---

- 说一说 JS 继承的方法和优点

> 原型链继承、构造函数继承、组合继承、原型式继承、寄生式继承、寄生组合式继承、ES6 Class 标准

**原型链继承**

优点：写法方便简洁，容易理解。

缺点：在父类型构造函数中定义的引用类型值的实例属性，会在子类型原型上变成原型属性被所有子类型实例所共享。同时在创建子类型的实例时，不能向超类型的构造函数中传递参数。

> 原型与原型对象是同一个东西

```js
function Father() {
  this.fatherObj = {pro: 'haha'}
  this.fatherStr = "father"
}
function Son() {
  this.sonProps = 'son'
}
Son.prototype = new Father()

const son = new Son()
```

**构造函数继承**

在子类型构造函数内部调用父类型的构造函数，通过 apply()或者 call()方法将父对象的构造函数绑定到子对象上。

优点：解决原型链继承不能传递参数问题和父类的原型共享问题。

缺点：借用构造函数的缺点是方法都在构造函数中定义，无法实现函数的复用。**在父类型的原型中定义的方法，对子类型而言也是不可见的**，结果所有类型都只能使用构造函数模式。

```js
function Father(name) {
  this.name = name
}
Father.prototype.fatherMethod = () => {
  console.log('father method')
}
function Son(name) {
  Father.call(this, name)
  this.subName = 'son'
  this.sunMethod = () => {
    console.log('son')
  }
}
const son = new Son('father name')
```

> 子类中是找不到`fatherMethod`方法的。

**组合继承**

将原型链继承与借用构造函数继承组合到一块，**使用原型链实现对原型属性与方法的继承，通过构造函数实现对实例属性的继承**。即实现了函数复用，有保证每个实例都有自己的属性。

优点：解决了原型链继承与借用构造函数继承造成的影响。

缺点：无论什么时候，都需要调用两次父类型构造函数；第一次在创建子类型原型的时候，第二次在子类型内部调用构造函数的时候。

```js
function Father(name) {
  this.fatherName = name
  this.fatherStr = "father"
}
Father.prototype.fatherMethod = () => console.log('father')

function Son(name) {
  Father.call(this, name)
  this.sonName = 'son name'
}
Son.prototype = new Father()
Son.prototype.sonMethod = () => console.log('son')

const son = new Son('hhh')
```

**原型式继承**

在一个函数 A 内部创建一个**临时性构造函数**，然后将传入的对象作为这个构造函数的原型，最后返回这个临时类型的一个新实例。本质上，函数 A 是对传入的对象执行了一次浅复制。ES5 通过`Object.create`方法将原型式继承的概念规范化。该方法传入两个参数，作为新对象原型的对象，以及给新对象定义额外属性的对象（可选）。只有一个参数时，效果与函数 A 相同。

优点：不需要单独创建构造函数

缺点：属性中包含的引用值始终会在**相关对象间共享**。

```js
function A(o) {
  function temp() {}
  temp.prototype = o
  return new temp()
}

const Father = {
  fatherName: 'father',
  shareObj: ['1', '2']
}

const son = A(Father)
```

**寄生式继承**

寄生式继承背后的思路类似于寄生构造函数和工厂模式：创建一个实现继承的函数，以某种方式增强对象，然后返回这个对象。

优点：写法简单，不需要单独创建构造函数。

缺点：通过寄生式继承给对象添加函数会导致函数难以复用。

> 在原型式继承的基础上增强浅复制的能力。只是一种思路

```js
function anotherA(o) {
  let clone = A(o)
  clone.sayHi = function () {
    // 增强浅复制能力
    console.log('hi')
  }
  return clone
}
let father = {
  name: 'father'
  share: ['1', '2']
}
let son = anotherA(father)
son.sayHi()
```

**寄生组合式继承**

通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。本质上，就是使用寄生式继承来继承父类型的原型，然后再将结果指定给子类型的原型。

优点：高效率，只调用一次父构造函数，避免在子原型上创建多于属性。原型链还能保持不变。

缺点：代码复杂。

```js
function inheritPrototype(son, father) {
  const proto = A(father.prototype)
  proto.constructor = son
  son.prototype = proto
}
function Father(name) {
  this.fatherName = name
}
Father.prototype.fatherM = () => console.log('f')
function Son(name) {
  Father.call(this, name)
  this.sonName = 'son'
}
inheritPrototype(Son, Father)

const son = new Son('hhh')
```

**ES6 Class 实现继承**

---

- 说一说`new`会发生什么

1. 创建一个简单空对象`{}`

2. 为空对象添加对象原型，指向构造函数的原型对象

3. 将空对象作为`this`的上下文

4. 如果函数没有返回对象就返回`this`

```js
function MyNew(...args) {
  const constructor = args.shift()
  const obj = Object.create(constructor.prototype)
  const result = constructor.apply(obj, args)
  return typeof result === 'object' && result !== null ? result : obj
}

function A(name) {
  this.name = name
}
const res = MyNew(A, 'jack')
```

> new 关键字后面的函数不能是箭头函数

---

- 说一说伪数组和数组的区别

伪数组的类型不是 Array，而是 Object，而数组类型是 Array。能够通过 length 属性查看长度，也可以使用`[index]`方式获取某个元素，**但不能够使用数组的其他方法，也不能够改变长度。遍历使用`for in`方法**。

为数组常见的场景：

1. 函数的参数`arguments`

2. 原生 js 获取 DOM：`document.querySelector('div')`等

3. jequery 获取 DOM：`$('div')`等

伪数组转化成真数组：`Array.prototype.slice.call(fake)`, `[].slice.call(fake)`, `Array.from(fake)`

---

- 说一说 axios 的拦截器原理及应用

> 请求拦截器，响应拦截器，Promise 控制执行顺序，每个请求带上相应的参数，返回的状态进行判断

请求(request)拦截器：在向接口请求前进行处理，例如为每个请求带上相应的参数（token 等）

响应(response)拦截器：用于在接口返回数据之后进行处理。例如判断返回状态，token 是否过期。

> Axios 提供的拦截器 API 分为：请求拦截器与响应拦截器

**拦截器原理**

> 内部实现的方法，同时，由于两个拦截器都是成对存在的，在`dispatchRequest`后面常用`undefined`来进行占位。

创建一个数组，数组中保存拦截器相应方法与`dispatchRequest`（用于下发请求）。请求拦截器方法放在`dispatchRequest`方法前，响应拦截器方法放在`dispatchRequest`方法后。遍历所有的请求拦截器与响应拦截器，分布`unshift`，`push`到数组中，为保证执行顺序，使用`promise`。

> 官方用法

```js
axios.interceptors.request.use((config) => {
  // some thing
  return config
}, (error) => {
  // 对请求错误的做法
  return Promise.reject(error)
})

axios.interceptors.response.use((response) => {
  // some thing
  return response
}, (error) => {
  // response error
  return Promise.reject(error)
})
```

---

- 说一说创建 ajax 过程

```js
// create xhr object
const xhr = new XMLHttpRequest()
// set request param
xhr.open('get', 'urlxxx')
// send request
xhr.send()
// post
// xhr.send(data)
xhr.onreadystatechange = function () {
  if (xhr.readyState == 4 && xhr.state == 200) {
    console.log(xhr.responseText)
  }
}
```

---

- 说一下 fetch 请求方式

> 原生 JS，没有使用 XMLHttpRequest 对象，头部信息，请求信息，响应信息等均匀分布到不同的对象

fetch 是一种 HTTP 数据请求的方式，是 XMLHttpRequest 的一种代替方案。是原生的 JS，`fetch()`方法返回一个 Promise。

**XMLHttpRequest 的问题**

1. 所有的功能集中在一个对象上，容易书写混乱，不容易维护代码。

2. 采用传统的事件驱动模式，无法适配新的 Promise API

**Fetch API 特点**

1. 精细的功能分割：头部信息，请求信息，响应信息等均匀分布到不同对象。更利于处理各种复杂的数据交互场景。

2. 使用 Promise API，更利于异步代码的书写

3. 同源请求也可以自定义不带 cookie。

---

- 前后端实时通信的方法

> 轮询、长轮询、iframe 流、WebSocket、SSE

**轮询**

服务器与客户端之间每隔一段时间就进行一次询问，缺点是连接数多，每次发送请求都会有 Http 的 Header。消耗流量，cpu 利用率。轮询时间长，用户无法及时更新数据，过短，则请求数量过多。优点是实现简单。

**长轮询**

针对轮询的改进，客户端发送 HTTP 给服务器后，如果没有新消息，就一直等待。有新消息，才会返回给客户端。某种程度上减小了网络带宽和 cpu 利用率的问题。**由于 HTTP 数据包的头部数据量往往很大（通常 400 多字节），但真正被服务器需要的数据却很少（有时只有 10 个字节左右），这样的数据包在网络上周期性的传输，浪费网络带宽**。同时，服务器没有返回有效数据，程序会超时。优点是做了优化，有较好的时效性。

**iframe 流**

在页面中插入一个隐藏的 iframe，利用其 src 属性在服务器和客户端之间创建一条长连接，服务器向 iframe 传输数据（通常是 HTML，内部有负责插入信息的 JS），来实时更新页面。优点是消息能够实时到达，浏览器兼容好。缺点是服务器维护一个长连接会增加开销。IE、chrome、Firefox 会显示加载没有完成，图标不停选择。

**WebSocket**

类似 socket 通讯模式，一旦 WebSocket 连接建立之后，后续数据都以帧序列的形式传输。在客户端断开 WebSocket 连接或服务端断开连接之前，不需要客户端与服务器重新发送连接请求。

在海量并发和客户端与服务器交互负载流量大的情况下，极大的节省了网络带宽的资源消耗，有明显性能优势，全双工。缺点是浏览器支持程度不一样，不支持断开重连。

**SSE**

server-Sent Event 是建立在浏览器与服务器之间的通信渠道，**服务器向浏览器推送信息，是单向通道**。本质是下载，优点是使用 HTTP 协议，现有服务器软件都支持。SSE 属于轻量级，使用简单；默认支持断开重连。

> 轮询适用于小型应用，长轮询适用早期 web IM 聊天，iframe 适用客服通信等，WebSocket 适用于微信、网络互动游戏等。SSE 适用于金融股票数据、看板等。

---

### 浏览器

- 浏览器的垃圾回收机制

> 栈垃圾回收、堆垃圾回收、新生区老生区、Scavenge 算法、标记-清除算法、标记-整理算法、全停顿、增量标记

浏览器垃圾回收机制根据数据的存储方式分为：栈垃圾回收，堆垃圾回收。栈垃圾回收方式简单，当一个函数执行结束后，JS 引擎通过向下移动 ESP(当前执行状态的指针)来销毁该函数保存在栈中的执行上下文。先进后出原则。

**堆垃圾回收**：当函数直接结束后，栈空间处理完了，但堆空间的数据虽然没有被引用，也还是存储在堆空间中。此时，需要垃圾回收器将堆空间中的垃圾数据回收。

为了使垃圾回收达到最好的效果，**根据对象的生命周期，使用不同的垃圾回收算法**。

V8 引擎中，将堆分为**新生代**与**老生代**两个区域，新生代中存放的是生存时间短的对象，老生代中存放的是生存时间久的对象。

新生区中使用**Scavenge 算法（清除算法）**，老生区使用**标记-清除算法与标记-整理算法**。

**Scavenge 算法**

1. 标记：对对象区域中的垃圾进行标记

2. 清除垃圾数据与整理碎片化内存

   > 副垃圾回收器会将存活的对象复制到空闲区域，并有序排列，复制后的区域就没有内存碎片。

3. 角色翻转：完成复制后，对象区域与空闲区域进行角色翻转——原来的对象区域变成空闲区，赋值后的空闲区与变成对象区域。至此完成垃圾回收，同时，这种角色翻转操作能够让新生代中的两块区域无限重复使用

**标记-清除算法**

1. 标记：从一组根元素开始，递归遍历该组根元素，在遍历过程中，能够达到的元素称为活动对象，没有到达的元素判断为垃圾数据，就是运行环境中的变量没有引用这些元素了。标记活动对象。

2. 清除：将垃圾数据进行清除

3. 产生内存碎片：对一块内存多次执行标记-清除算法后，会产生大量不连续的内存碎片，而碎片过多将导致大对象无法分配到足够的连续内存。

**标记-整理算法**

1. 标记：同标记-清除算法

2. 整理：让所有存活的对象都向内存的一端移动

3. 清除：清理掉端边界以外的内存

**V8 引擎使用副垃圾回收器与主垃圾回收器进行垃圾回收，但是，由于 JS 是运行在主线程之上的，一旦执行垃圾回收算法，都需要将正在执行的 JS 脚本暂停下来，带垃圾回收完后再恢复脚本执行。这种行为叫全停顿**。

为了降低**老生代**垃圾回收造成的卡顿，V8 将标记过程分为一个个子标记过程，同时让垃圾回收标记和 JS 应用逻辑交替执行，直到标记阶段完成。把这个算法称为**增量标记(Incremental Marking)算法**。

> 垃圾回收机制只是清除局部变量——也就是函数内部的变量，函数外部的变量基本是全局变量，只有在页面被关闭时才会被清理。

**扩展**

在 V8 引擎中，被限制了内存的使用大小——64 位约 1.4G/1464M，32 位约 0.7G/732M。

通常，JS 中大多数对象的存活周期较短，大部分经过一次垃圾回收后就被释放。为了提高回收率，才将堆分成新生代与老生代两个区域。

新生区通常只支持 1~8M 的容量，而老生区可支持大容量，对这两个区域分别使用不同的垃圾回收器进行垃圾回收。

1. 副垃圾回收器 - Scavenge：负责新生代垃圾回收

2. 主垃圾回收器 - Mark-Sweep & Mark-Compact：负责老生代垃圾回收

> Savenge 算法是牺牲空间换时间的算法，将新生代划分成两个区域`from-space`与`to-space`。JS 中任何对象的声明所有获取的空间都会放在`from-space`空间中。

**垃圾回收机制如何知道对象是活动与非活动**？

涉及对象可达概念，从初始根对象（window，global）的指针开始，这个根指针对象被称为根集（root set），从该根集向下搜索其子节点，被搜索到的子节点说明该节点的引用对象可达，并为其留下标记。那么未被标记的内存对象表示不可达，需要被回收。

**新生代中的对象什么时候变成老生代对象**？

实际上，新生代中，进行了进一步划分，分为`nursery(幼儿园)`子代，`intermediate(中学)`子代两个区域，所有对象第一次分配内存会被放到`nursery`子代中，如果经过下一次垃圾回收后，对象还存在，就放入`intermediate`子代中，再进过下一次垃圾回收后，若对象还存在新生代中，副垃圾回收器就会将该对象放入老生代中。_该移动过程称为晋升_。

---

- CSRF 攻击是什么？

> 跨站点请求伪造，盗用用户身份发起请求，Cross-site request forgery，XSS——cross-site scripting 跨站脚本攻击

> 主要利用 cookie 进行攻击

跨站点请求伪造，与 XSS 一样，有着巨大的危害。攻击者盗用用户身份，以用户的身份发送恶意请求，但该请求对服务器是合理的。

**原理**：

用户打开浏览器，访问目标网站 A，输入用户名与密码请求登录；用户信息在通过认证后，网站 A 产生一个 cookie 信息返回给浏览器。此时，用户可正常发送请求到网站 A

**当用户没有退出网站 A 时**，在同一浏览器打开网站 B，网站 B 收到用户请求后，返回一些攻击代码，同时发出请求(例如自动请求的表单)，要求访问网站 A。浏览器收到这些攻击代码后，根据网站 B 的请求在用户不知情的情况下以用户的权限操作了 cookie 并向网站 A 服务器发起合法请求。

**预防手段**

1. 使用验证码：在表单中添加随机数字或字母，强制用户与应用进行交互

2. HTTP 中 Referer 字段：检查是否从正确的域名访问过来，该字段记录 HTTP 请求的来源。

3. 使用 token 验证：在 HTTP 请求中添加 token 字段，并在服务器设立拦截器验证 token，若 token 不对，则拒绝请求。

> Referer 实现简单，同时无需修改现存的代码逻辑，但不同浏览器实现该字段的方式不一样，这等同于将安全性交给浏览器实现。使用 token 则比 referer 更安全一点。将 token 放到 HTTP 自定的请求头中也解决了使用 get 与 post 方式传参的不便性。

- XSS 攻击是什么？

> 跨站点脚本攻击，向目标网站插入恶意代码，大量用户访问网站时运行恶意脚本获取信息。

XSS 为跨站点脚本攻击 Cross-site scripting，不写成 CSS 是为了与样式文件缩写进行区分，攻击者可以通过向 Web 页面里插入`script`代码，当用户浏览这个页面时，就会运行被插入的代码，达到攻击者的目的。

> 页面中，只要是在`script`标签内地东西都会被当做正常的 js 脚本执行，常见的情况为用户输入评论时，若没有对输入过滤，攻击者就可以将评论写成`<script>console.log('attack')</script>`，一旦该评论被服务器放到网页上，就会在控制台输出 attack。

XSS 一般的危害为泄露用户的登录信息 cookie，攻击者可以通过 cookie 绕过登入步骤直接进入站点。

XSS 通常分为反射型与存储型。反射型是临时通过 url 访问网站，网站服务端将恶意代码从 url 中取出，拼接在 HTML 中返回给浏览器，用户就会执行恶意代码。存储型就是将恶意代码以留言的形式保存在服务器数据库中，任何访问网站的人都会收到攻击。

预防 XSS 的基本方案就是对数据进行严格的输出编码，如 HTML 元素编码，JS 编码，css 编码，url 编码等。

**扩展**

1. 获取 cookie： 网站登录常用 cookie 作为某个用户身份的验证，是服务器返回的一串字符，可以用来绕过密码登录，当空间、论坛等地方能够被插入`script`代码时，所有用户都会面临攻击。

2. 恶意跳转：页面中插入`window.location.href`进行跳转

3. 反射型 xss：get 方法

4. 存储型 xss：post 方法

防御 XSS 攻击：浏览器的防御与"X-XSS-Protection"有关，默认为 1，开启防御。可以预防反射型 xss，但作用有限，只能防御注入到 HTML 节点内或属性上的 xss，如 url 上有 script 标签。

预防 HTML 节点内容的 xss 攻击：对标签符号进行转义。属性预防：对双引号进行转义（要求属性必须带引号）。预防 JS 代码：将数据进行 JSON 序列化。

防御富文本：较为复杂，通过白名单方式过滤允许的 HTML 标签与属性进行防御。

开启浏览器 XSS 防御，禁止 JS 读取某些敏感 cookie。

---

- 说一说 defer 和 async 区别。

> 加载 JS 文档和渲染文档可以同时进行、JS 代码立即执行、JS 代码不立即执行、渲染引擎和 JS 引擎互斥

浏览器会立即加载 JS 文件并执行指定的脚本，“立即”指的是在渲染该`script`标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行。

加上`async`属性，加载 JS 文档和渲染文档可以同时进行（异步加载）。当 JS 加载完成，JS 代码立即执行，会阻塞 HTML 渲染。

加上`defer`，同样的，加载后续文档元素过程与 JS 加载过程并行（异步），**当 HTML 渲染完成后，才会执行 JS 代码**。

**扩展**

渲染阻塞的原因：由于 JS 是可操作 DOM 的，如果在修改这些元素属性同时渲染界面（JS 线程与 UI 线程同时运行），那么渲染线程前后获得的元素数据就可能不一致。因此，为了防止渲染出现不可预期的结果，浏览器设置 GUI 渲染线程与 JS 引擎为互斥关系。

当浏览器执行 JS 程序时，GUI 渲染线程会被保存在一个队列中，直到 JS 程序执行完成，才会接着执行。如果 JS 执行时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

---

- 浏览器如何渲染页面

> dom 树，stylesheet，布局树，分层，光栅化，合成

浏览器拿到 HTML，先将 HTML 转化成 dom 树，再将 CSS 样式转换成 stylesheet，创建布局树(依据 dom 树结构，使用 css 样式生成的树形数据)，对布局树进行分层，为每个图层生成绘制列表，再将图层分成图块，紧接着光栅化，将图块转化成位图，最后合成绘制生成页面。

**扩展**

分层的目的：避免整个页面渲染，把页面分成多个图层，尤其是动画的时候，把动画独立出一个图层，渲染时只渲染该图层（transform, z-index 等）。浏览器会自动优化生成图层。

光栅化目的：页面如果很长，但可视区域很小，避免渲染非可视区域的样式造成资源浪费，所以将每个图层有划分成多个小格子，当前只渲染可视区域附近的内容。

---

- token 能放在 cookie 中吗？

> 不要与 JWT 混淆，token 是服务端生成的一串字符串，作为客户端请求的一个令牌，当第一次登录后，服务器生成一个 token 并将其返回给客户端，之后客户端只需带上 token 请求数据即可，无需再次带上用户名与密码，而服务器也只需在 token 库中查询匹配。可以减轻服务器压力。

可以，token 一般用来判断用户是否登录，内部包含的信息有：uid （用户唯一的身份标识）、time （当前时间的时间戳）、sign （签名，token 的前几位以哈希算法压缩成一定长度的十六进制字符串）

token 可以存放在 cookie 中，token 是否过期应该由后端来判断，而不是前端，所以 token 存在 cookie 中只要不设置 cookie 的过期时间就可以（否则 cookie 会被删除）。

如果 token 失效，就让后端返回固定的状态，表示 token 失效，需要重新登录，重新登录后，重新设置 cookie 中的 token 即可。

**扩展**

token 认证流程

1. 客户端使用用户名跟密码请求登录

2. 服务端收到请求，验证用户名与密码

3. 验证成功后，服务端签发一个 token，并发送给客户端

4. 客户端收到 token 以后会把它存储起来，例如放在 cookie 中或者 localStorage 中。

5. 客户端每次发送请求时都需要带着服务端签发的 token （放在 HTTP 的请求头里）

6. 服务端收到请求后，需要验证请求里带有的 token，如果验证成功，则返回对应的数据。

---

- 说一说浏览器输入 URL 发生了什么

输入地址，判断是否是 HTTP 缓存，如果是强制缓存且在有效期内，不再向服务器发请求，浏览器查找域名 IP 地址，向该 IP 地址的 web 服务器发送一个 HTTP 请求，在发送请求之前，浏览器与服务器建立 TCP 的三次握手，如果是 HTTP 协商缓存，向后端发送请求且和后端服务器对比，在有效期内，服务器返回 304，直接从浏览器获取数据，如果不在有效期内，服务器返回 200，返回新数据。

请求发送出去，服务器返回重定向，则浏览器按照重定向的地址重新发送请求。如果请求参数有问题，服务端返回 400，如果服务器挂了，放回 500。

如果数据一切正常，当浏览器拿到服务器的数据后，开始渲染页面同时获取 HTML 页面中图片、音频、视频、CSS、JS，在这期间，获取到 JS 文件后，会立即执行 JS 代码，会阻塞浏览器渲染，因为渲染引擎与 JS 引擎互斥，所以通常把`script`标签放在`body`标签的底部。

渲染过程是先将 HTML 转换成 dom 树，再将 css 样式转化成 stylesheet，根据 dom 树与 stylesheet 创建布局树，对布局树进行分层，为每个图层生成绘制列表，再将图层分成图块，接着光栅化，将图块转化成位图，最后合成绘制生成页面。

> URL 解析、查找缓存、DNS 域名解析、TCP 三次握手、发起 HTTP 请求、服务器响应并返回结果、TCP 四次握手、浏览器渲染、js 引擎解析。

---

- 说一说如何实现可过期的`localstorage`数据

> 惰性删除、定时删除

`localStrorage`只能用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除。所以要实现可过期的`localStorage`缓存的重点就是**如何清理过期的缓存**。目前，常用的方法分为两种：惰性删除；定时删除。

惰性删除：是指某个键值过期后，该键值不会被马上删除，而是等到下次被使用的时候，才会被检查到过期，此时才能得到删除。**实现方法：存储数据类型为对象，包含两个`key`，一个存储数据的值，另一个是当前时间**。获取数据的时候，拿到存储的时间和当前时间做对比，如果超过过期时间就清除。(牛课上说清除 cookie？)

定时删除：每隔一段时间执行一次删除操作，并通过限制删除操作执行的次数和频率，来减少删除操作对 CPU 的长期占用。另一方面，定时删除也有效减少了因惰性删除带来的对`localStorage`空间的浪费。**实现过程：获取所有设置过期时间的`key`，判断是否过期，过期就存储到数组中，遍历数组，每隔 1s（固定时间）删除 5 个（固定数），直到把数组中的`key`从`localStorage`中全部删除**。

> 使用场景，token 存储在 localStorage 中，需要清空。注意，两种方式都需要设置时间，所谓的减少空间浪费是定时删除可以及时清理无用项。

---

- 说一下重绘、重排区别如何避免？

> 改变元素尺寸、重新计算布局树、重排必定重绘、重绘避开了重建布局树和分层、GPU 加速、脱离文档流、样式集中改变

**重排（回流）**

当 DOM 的变化影响了元素的几何信息（位置，尺寸大小），浏览器需要重新计算元素的几何属性，并放置在正确的位置，该过程为重排。

**重绘**

当一个元素的外观发生改变，但没有改变布局，重新把元素外观绘制出来的过程，所以**重绘跳过了创建布局树和分层的阶段**。

重排需要重新计算布局树，重绘不需要，重排必定发生重绘，重绘不一定要重排。因此，重排对性能消耗更多。

触发重排的方法：

1. 页面初始渲染

2. 添加、删除可见的 dom 元素

3. 改变元素位置

4. 改变元素尺寸：编剧，填充，边框，宽度，高度等

5. 改变元素内容：文字数量，图片大小等

6. 改变元素字体大小

7. 改变浏览器窗口尺寸

8. 激活 cc 伪类：`:hover`

9. 设置`sytle`属性的值：通过设置该元素属性的值，每次都会触发重排

10. 查询某些属性或调用某些计算方法：`offsetWidth`等

**避免重排方式**

1. 样式集中改变

2. 使用`absolute`或`fixed`脱离文档流

3. 使用 GPU 加速`transform`

**扩展**

GPU 加速过程

1. 获取 DOM 并分割成多层（renderLayer）

2. 将每个层栅格化，**独立的**绘制进位图

3. 将这些位图作为纹理上传至 GPU

4. 复合多个层生成最终的屏幕图像

开启了 GPU 加速的元素被独立出来，不会在影响其他 DOM 的布局，因为其改变后只是相当于被贴上了页面。

---

- 前端性能优化

前端性能有话分为两类：一类是文件加载更快，另一类是文件渲染更快。

加载快的方法：

1. 降低数据包大小：图片压缩，文件压缩

2. 减少网络请求次数：雪碧图，精灵图

3. 节流防抖：减少渲染次数

4. 缓存：HTTP 缓存，本地缓存

渲染更快的方法：

1. ssr 服务端渲染

2. 避免渲染阻塞：css 放在 HTML 的 head 中，JS 放在 body 底部

3. 避免无用渲染：懒加载

4. 减少渲染次数：对 dom 查询进行缓存，合并都没操作，减少重排的标签

> 雪碧图应用场景一般是项目中不常更换的一些固定图标组合在一起，比如 logo，搜索图标，切换图标等。电商项目中最常用到的懒加载。

---

- 说一说性能优化有哪些指标，如何量化

常用的性能优化指标

1. Speed Index（lighthouse，速度指标）

2. TTFB（Network，第一个请求响应时间）

3. 页面加载时间

4. 首次渲染

5. 交互动作的反馈时间

6. 帧率 FPS（动画`ctrl+shift+p`）

7. 异步请求完成时间

使用性能测量工具进行量化

1. Chrome DevTools

2. 开发调试、性能测评

3. Audit（Lighthouse）

4. Throttling 调整网络吞吐

5. Performance 性能分析

6. Network 网络加载分析

7. Lighthouse

8. 网站整体质量评估

> 总结：1.性能评估 Chrome Performance 选项卡/Lighthouse 生成性能检测报告。2.值得关注的性能指标：1）LCP（Largest Contentful Paint 最大内容绘制）。2）首屏渲染时间（白屏时间）。3）FCP（First Contentful Paint 首先内容绘制）。4）可交互时间（Time to Interactive TTI）。5）Network 请求时间。3.浏览器开发者工具可以看到很多信息，调用性能监测 API 或建立前端监控系统（无痕埋点）。

---

- 说一说服务端渲染

> 服务端生成 HTMl 直接返回给浏览器，减少网络传输，首屏渲染快，对搜索引擎友好

SSR 为 server side render 简称，页面上的内容是通过服务端渲染生成的，浏览器直接显示 HTML 就可以。与之对应的是 CSR（client side render）。客户端请求时，服务端不做任何处理，直接将前端资源打包后生成的 html 返回给客户端。需要客户端运行 JS 代码才能渲染生成页面内容，同时完成事件绑定，客户端通过 ajax 请求后端 api 获取数据。

服务端渲染优势：减少网络传输，响应快，用户体验好，首屏渲染快，对搜索引擎友好（搜索引擎爬虫可以看到完整的程序源码，有利于 SEO）。

**扩展**

使用 SSR 时，需要权衡一些利弊。

1. 开发条件限制。浏览器特定的代码，只能在某些生命周期钩子函数中使用；一些外部扩展库可能需要特殊处理才能在服务器渲染应用程序中运行。

2. 涉及构建设置和部署的更多要求。与可以部署在任何静态文件服务器上的完全静态 SPA 不同，服务器渲染应用程序需要处于 NodeJS 运行环境。

3. 更多服务器端负载。在 NodeJS 中渲染完整的应用程序，显然会比仅仅提供静态文件的 server 更加大量占用 cpu 资源，如果在高流量的环境中，需要配备相应的服务器负载。

---

### React

- react 生命周期

> 只有类组件才会有生命周期，函数组件可用通过 `useEffect` 来模仿生命周期

React 生命周期中，常用的有 `constructor` 负责数据初始化，`render` 将 jsx 转换成真实 dom 节点。`componentDidMount` 组件第一次渲染完成时触发。`componentDidUpdate` 组件更新完成时触发。`componentWillUnmount` 组件销毁和卸载时触发。不常用的方法有：`getDerivedStateFromProps` 从 props 中更新 state 和处理回调。`shouldComponentUpdate` 用于性能优化，控制组件是否更新。`getSnapshotBeforeUpdate` 代替了 `componentWillUpdate` 函数。

- React 组件之间传值的方法

> props, context, 发布/订阅

React 中存在多种传递数据的方式，按照不同的组件关系可以分为**父子组件传值**，**跨级组件传值**，**非嵌套关系组件传值**。

父子组件常用的传值方法：父组件通过 props 给子组件传值，子组件通过回调函数给父组件传值。

跨级组件常用的传值方法：使用 props 层层传递，使用 context 对象，将父组件设置为生产者，而子组件都设置对应的 contextType 即可。

非嵌套关系组件传值的方法是使用共同的父组件进行 props 传值，或者通过 context 传值，也可以使用发布/订阅的自定义事件进行传值。

---

- setState 是同步还是异步

在合成事件与生命周期中是异步的，在原生事件和定时器中都是同步的。

**扩展**

`setState`本身不分同步与异步，而是取决于是否处于批量更新中。组件中所有的函数在执行时，临时设置一个变量`isBatchingUpates = true`，当遇到`setState`时，如果是批量更新，就是异步。如果是非批量更新，就是同步。

**什么时候批量更新是关闭的**

1. 函数运行结束时，`isBatchingUpdate=false`

2. 当函数遇到`setTimeout`，`setInterval`时，`isBatchingUpdate=false`

3. 当 dom 添加自定义事件时

> 总结：**同步代码异步表现，原因是 react 批处理，react18 之前，在生命周期与合成事件中表现为异步，在原生事件表现同步，react18 后优化了批处理，任何地方调用 setState 都表现为异步**。

> 原生事件：在生命周期里进行事件绑定。合成事件：在 JSX 里面绑定事件。

---

- React 事件绑定原理

在 React 中，event 事件不是原生事件，而是对原生 event 进行了封装的新类`SyntheticBaseEvent`(合成基础事件)。模拟出 DOM 事件的所有功能，通过`event.nativeEvent`可以获取到原生事件。

从 react17 开始，所有事件都是绑定在 root 根组件上，之前都是绑定在 document 上。React 中的 event 与 dom 事件，vue 中的都不一样。

**扩展**

将事件绑定到 root 处，当事件冒泡到 root 处时，将事件内容进行封装，传递给处理函数运行。不仅减少了内存消耗，还能在组件挂载销毁是统一订阅与移除事件。同时冒泡到 root 的事件也是 react 自己的合成事件（SyntheticBaseEvent），因此想要阻止事件冒泡，需要使用`evnet.preventDefault`，而不是`event.stopPropagation`。

---

- react 中 hooks 的优缺点

1. 代码可读性强，在使用 hooks 之前，发布、订阅，自定义事件被挂载在`componentDidMount`生命周期中，需要在`componentWillUnmoutn`中进行清除，不利于开发与维护，使用 hooks 后`useEffect`将三个生命周期聚合起来，方便维护。

2. 组件层级变浅，使用 hooks 之前，常使用高阶组件（HOC）的方法来复用多个组件的公共状态，加大了组件渲染开销。在 hooks 中可以自定义组件`useXxx`方法将多个组件之间共享的逻辑放入自定义 hook 中。

3. 不需要考虑`this`指向问题。

缺点：只有模拟了三个生命周期，对于`getSnapshotBeforeUpdate`与`componentDidCatch`等其他生命周期没有支持。

---

### 代码

事件循环中，`var`与`let`会后不同的结果，网上说法为，let 改变了原本的作用域链形式，函数作用域变成块级作用域。效果如下：

```js
for(var i = 0; i < 6; i++) {
  setTimeout(() => {
    console.log(i)
  }, 1000)
}
// print 6 6 6 6 6 6

for(let i = 0; i < 6; i++) {
  setTimeout(() => {
    console.log(i)
  }, 1000)
}
// 0 1 2 3 4 5
```

### others

- 浏览器的渲染过程

1. 浏览器获取到 html 资源并解析 DOM 树

2. 解析到 css 后生成 css 规则树（style rules）

3. 当 dom 树与 css 规则树都生成后，通过两者生成渲染树（render tree）

4. 渲染树构建完成后，浏览器开始计算元素的大小和位置（layout）

5. 根据计算的节点信息，将内容绘制到屏幕上（painting）

#### JWT

JWT(json web token)出现的原因是因为服务器发给浏览器的信息是存在浏览器本地的，当服务器再次收到浏览器中附带的消息时，无法验证该消息的正确性（是否被修改，伪造等）。

为了解决上面的问题，可以在服务器设置一个秘钥，将发送给浏览器的验证信息通过该秘钥进行加密，将得到的结果与验证信息一起发给浏览器，当后续收到浏览器的附带信息时，将该信息通过秘钥进行一次加密，若得到的结果与附带信息中的结果不同，则认为该信息不可信。

> 设浏览器发送的消息为 A，将 A 通过加密，得到 key。将 A.key 发送给浏览器，后续收到的浏览器发来的 A.key 时，先对 A 进行加密，若得到的 key，与 A.key 中的 key 不相同，则认为信息被篡改。

而 JWT 就是执行上面这个操作的一套标准，分为三部分，每个部分通过`.`分割。

1. header：记录本次 JWT 使用的加密算法，类型等，基本是不变的（常用的是 HS256 算法），是一个对象，采用 base64 编码

2. playload：一些用户信息（不要放敏感信息），采用 base64 编码

3. Signature：前面两部分通过秘钥加密后的结果，同样采用了 base64 编码

> 与 session 的区别：session 是存储在服务器中，JWT 存储在客户端中。

#### 浏览器的缓存机制

> 浏览器可以根据相应报文中的缓存标识决定是否将内容缓存。是则将请求结果与缓存标识存入浏览器。

1. 每次请求前，在缓存中查找是否请求结果与缓存标识

2. 没有则发送新的请求，每次得到新的请求结果，都将结果与缓存标识存入缓存中。
