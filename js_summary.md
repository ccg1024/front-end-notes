---
title: Javascript 总结
desc: 将js的笔记与面试内容进行总结
date: 2023-5-29
---
# 基础

Js 的基础数据类型：`boolean`, `string`, `number`, `bigint`, `undefine`, `symbole`, `null`, `object`。

> NaN 是一种特殊的 nubmer 类型。

常见的`Array`方法：

- `push`, `pop`, `shift`, `unshift`

- `slice`

- `splice`

- `map`

- `forEach`

- `filter`

- `reducer`

常见的修改`this`指向的函数：`apply`, `call`, `bind`。

使用指定对象作为原型的方法：`const newObj = Object.create(obj)`

## getters/setters

能够让对象看上去具备某个属性，实际上是两个函数。

```js
const o = {
  pro: 'leon',
  get name() {
    return this.pro
  },
  set name(name) {
    this.pro = name
  }
}

console.log(o.name)
o.name = 'ada'
```

## new

该关键字进行的操作：

1. 创建一个空的简单 JS 对象`{}`

2. 为该对象添加属性`__proto__`，同时，将对象原型指向构造函数的原型

3. 将该对象作为`this`的上下文

4. 如果`new`后面的函数没有返回值，则返回`this`

```js
function MyNew(...args) {
  const constructor = args.shift()
  const obj = Object.create(constructor.prototype)
  const res = constructor.apply(obj, args)
  return typeof res === 'object' && res !== null ? res : obj
}
```

## 深浅拷贝

浅拷贝常见方法：`Object.assgin()`, `{...obj}`, `Array.prototype.concat()`, `[...arr]`, `Array.protorype.slice()`。

深拷贝常见方法：`lodash.clonDeep`， `JSON.stringify()`，递归拷贝。

> 注意，JSON 方法无法拷贝对象中的函数值。

# 闭包

由函数与该函数的词法环境构成，该环境包含的创建闭包时，作用域内的任何变量，**会存在内存泄露的风险**。可以用来封装数据与隐藏。

# 事件

事件监听的三要素：事件源，事件类型，事件处理函数

事件流分为：捕获阶段与冒泡阶段，捕获为从上到下，冒泡为从下到上。默认情况下，事件监听是在冒泡阶段。如果在捕获阶段就执行监听函数，需要设置第三个参数。

```js
document.addEventListener('scroll', handler, true)
```

# event loop

单线程的 JS 实现异步的方式，首先，在主线程中，运行**执行栈**中的同步代码，碰到异步代码时，将异步任务提交给异步处理进程。执行栈继续处理后续的同步代码。在异步处理进程中，异步任务完成后，将异步任务按序推入**任务队列**。当**执行栈**为空时，查询任务队列中是否有异步任务，有则推出一个异步任务到执行栈中处理。

若再将异步任务细分成**宏任务**与**微任务**时：同样，主线程按序运行同步代码，遇到宏任务，则将该任务压入宏任务队列，遇到微任务时，将微任务压入当前任务的微任务队列，执行完主线程中的所有同步代码后，将查询当前任务中的微任务队列，有则按序执行微任务，没有，则查询宏任务队列，并推出第一个宏任务至主进程。

**常见的宏任务**

1. 执行`script`标签内的代码

2. `setTimout/setInterval`

3. `ajax`

4. `postMessage/MessgeChannel`

5. `setImmediate`

6. `I/O(nodeJS)`

7. DOM event

**常见的微任务**

1. `Promise`

2. `MutationObserver`

3. `Object.observe`

4. `process.nextTick`

# 浏览器

浏览器中`cookie`一般很小，只能够存储 4k 左右的内容，但内容大于 4k 时，多于的内容会被裁剪。为了解决这个问题，同时避免某些不应该用`cookie`存储的内容却用了的局面。H5 新增了`locaStorage`与`SessionStorage`两个存储变量，大小为 5M 左右。

通常与`session`一起询问，`session`是存储在服务端的，用来确保连接性，因为`cookie`是无连接的。后者通常用于登录检测。

> 两个都只能存储字符串，对象会显示[Object Obejct]，**也无法跨域**。注意，没有跨域的情况是：协议，域名，端口必须一模一样，有差异就是跨域。

## localStorage

同域名下，多窗口共享，即便关闭浏览器，内容依旧存在，除非手动删除。

## SessionStorage

同域名下，单窗口共享，关闭浏览器窗口则消失。但是，在一个窗口上通过`windows.open`的方式打开另一个同域名窗口，当前窗口的`SessionStorage`内容会被带过去，但不共享。

# 字节面试准备

cookie，sessionStorage，localStorage 都是本地存储。共同特点是都存储在本地。区别在于：cookie 由服务器写入，而后两者由前端写入。cookie 的生命周期有服务器写入时设置好，localStorage 则是写入就一直存在。sessionStorage 页面关闭自动清除。cookie 较小，4kB，后两者较大 5M。

三者都遵循同源原则，而 sessionStorage 还限制在同一个页面。前端往后端发送请求时，会自动携带 cookie 中的数据，但后两者中的内容不会。需要手动设置。

cookie 一般用于存储登入验证信息 SessionID 或 token 的。localStorage 则用来存储不易变动的数据，减轻服务器压力。sessionStorage 则可以用来监测页面是否刷新。

会话 cookie：没有明确设置时间的 cookie 不会存储在本地硬盘里，而是保存在内存中。

持久 cookie：设置过时间的 cookie 会保存在本地硬盘中，可以在不同的浏览器之间共享。

实现 cookie 的机制是通过扩展 HTTP 协议来实现的，服务器通过在 HTTP 的响应头中加上一行特殊的指示来提示浏览器按照指示生成相应的 cookie，同样的 JS 脚本也可以生成 cookie。**浏览器会检查所有 cookie，当某个 cookie 的作用范围大于等于将要请求的资源位置时，就会将该 cookie 附在请求头中发送给服务器**。

cookie 内容主要包括：键值对，路径，域，过期时间。路径与域构成作用范围。

使用Ajax请求时，同源地址会自动携带cookie，对于跨域的内容需要设置属性`withCredentials: ture`，对应到HTTP请求头上的属性为`Access-Control-Allow-Credentials: true`，此时`xxx-Origin`不能使用通配符。

```js
// 创建一个配置好的请求对象。后续通过api对象发送的请求都会启动下面的配置
const api = axios.create({
  baseURL: 'https://www.backend.api/',
  withCredentials: true
})
```

由于 cookie 是存储在本地，可以被篡改，为进一步保证会话跟踪的安全性，产生了 session——存储在服务端。但是过多的 session 会给服务器带来内存压力。于是有 JWT，可以说是升级版的 cookie。

session 是服务器使用类似散列表的结构来存储信息。当程序需要给某个客户端请求创建一个 session 时，首先检查请求中是否包含一个 session 标识——sessionID。如果有，则标识已经为该用户创建过 session。提取并使用，没有则创建，并生成一个关联的 sessionID。该 ID 是一个不重复，且难以找到规律仿造的字符串。

存储 sessionID 可用 cookie。但是 cookie 可以被客户端禁止，因此产生其他技术来存储，例如**URL 重写**，将 sessionID 附加在 URL 路径后面。**表单隐藏字段**，服务器自动修改表单，以便表单提交时能够自动携带 sessionID。

> URL 重写就是将页面中所有的 URL 都动态的加上 sessionID，然后将内容发给用户——那么 SPA 应用就需要使用 SSR 或则 hybrid 方式。
>
> 同样的，表单隐藏字段也是在服务端给表单添加隐藏字段后，再将内容发送给客户端。

**高阶函数**：接受的参数中存在函数，或者返回一个函数，则该函数为高阶函数。

**函数柯里化**：通常配合高阶函数使用，连续接受参数，在最后一个函数中统一处理数据。

```js
function fn1(arg1) {
  return (arg2) {
    let sum = arg1 + arg2
    return (arg3) {
      return sum + arg3
    }
  }
}
var1 = fn1(1)
var2 = var1(2)
var3 = var2(3) // var3 == 6
```

**能够产生闭包的原理**：JS 中通过作用域链访问变量，会自动向上一级查找。所以才能够产生闭包这样的概念。

**promise**：异步微任务。promise 中其他方法：

- `Promise.all()`: 一个列表中所有 promise 都成功时，才会返回一个成功的 promise 对象，否则都会触发失败

- `Promise.any()`：只要有一个成功，返回的 promise 就是成功的。

- `Promise.race()`: 任意一个 promise 成功或失败了就会返回一个相应的状态的 promise，并附上子 promise 的返回信息。

```js
// new Promise((resolve, reject) => {})
const PENDING = 'pending'
const FULLFILED = 'fullfiled'
const REJECT = 'reject'
class MyPromise() {
  constructor(executor) {
    this.#status = PENDING
    this.#value = undefined
    try {
      executor(this._resolve.bind(this), this._reject.bind(this))
    } catch(err) {
      this.#reject(err)
    }
  }

  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) => {
      if (this.#status === FULLFILED) {
        onFulfiled(this.#value)
      } else if (this.#status === REJECTED) {
        onRejected(this.#value)
      }
    })
  }

  #resolve(data){
    this.#changeState(FULLFILED, data)
  }
  #rejecte(reason){
    this.#changeState(REJECT, reason)
  }
  #changeState(newState, value) {
    if (this.#status !== PENDING) {
      return
    }
    this.#status = newState
    this.#value = value
  }
}
```

跨域问题中，通过 cors 来设置允许跨域时，设计到达 HTTP 头部属性为：

- `Access-Control-Allow-Origin`: '\*'

- `Access-Control-Allow-Methods`: 'GET, PUT, OPTIONS, POST'

> 通过 res.setHeader(key, value)来设置。

node 中间件，nginx 反向代理：页面将请求发送给同源的代理服务器，代理服务器再向后端发送请求。并返回结果。

JSONP，postmessage（发送与接受信息的 API）

```html
<div>
  <script>
    function handleJSONP(data) {
      console.log(data)
    }
  </script>
</div>
<script src="https://www.data.api/jsonp?callback=handleJSONP"></script>
```

```js
// back-end

app.get('/jsonp', async (req, res) => {
  const callback = req.params.callback

  res.send(`${callback}('hhh')`)
})
```

> 通过 script 标签应该期望收到 js 脚本，所以 JSONP 要返回函数调用，上面的案例中，得到的结果为 handleJSONP('hhh')

BFC 常见的实现方式：

- overflow 为非 visible

- position 为 absolute 或 fixed

- display 为 flex 或者 inline-block 等

- float 设置为 left 会 right

JS 实现异步的方法：回调函数（基本），事件监听，`setTimeout`，Promise，生成器，`async/await`。

`null`实际上是有自己的类型的（Null）。但是 typeof 会判断为 Object 类型是应为在底层通过二进制判断，而 null 的二进制都是 0，所以被误判。

浮动的作用是让元素出现在其容器的左边或者右边，同时让文字与行内元素环绕周围，与脱离文档流的定位不同，浮动不会遮挡文档流元素。但是呢，浮动元素本身是脱离文档流的。

清除浮动的三种方法：伪元素，BFC，标签插入法

- '.clearOverflow::after'{content: '';clear: both;display:block}

- overflow: hidden

- 给浮动元素父级插入新的标签，基本不用

**HashRouter**与**HistoryRouter**区别。

两者都是通过浏览器对象实现，其中`historyRouter`通过浏览器的`history`对象记录的历史栈实现，`hashRouter`则是通过监听`location`对象的`hash`属性实现，在没有使用hashRouter的时候，hash值通常为空字符。

相同的url，historyRouter会触发添加到**浏览器历史记录栈**中，hashRouter不会。

同时，historyRouter需要后端配合，否则刷新页面会出现404错误。原因在于前端路由的实现方式：当前，使用前端框架VUE，React基本都是制作SPA应用——只有一个html模板文件，所有页面内容都是通过JS进行动态渲染。而浏览器对一个url发送请求，通常是在服务器中找这个url的html文件。所以，非主页内容刷新浏览器时，就会发送一个不存在html文件的请求地址，这时，需要后端配合返回主页的内容，让前端路由来动态渲染具体的页面。

而hashRouter本就基于文件系统的，hash值的改变不会刷新浏览器，但能够触发页面跳转。同样身为前端路由，不需要后端支持是因为url通过`#`进行划分，在一般的url构成部分中，`#`之后的内容是作为页内锚点跳转标识的，而前端路由中，`#`前的路径指向主页部分，所以不会出现404。

hashRouter通过`window.onhashchange`方法来获取url变化，historyRouter通过`history.pushState()`或`history.replaceState()`来实现页面跳转，同时通过`window.onpopstate`来监听前进与后退。

> 注意，pushState与replaceState是不会触发onpopstate事件的，只有在点击浏览器后退按钮，或js中调用history.back()才会触发。


**事件循环**：执行js代码时，遇见同步任务，则直接推入调用栈中执行，遇到异步任务，将该任务挂起，等到异步任务返回之后推入到任务队列，当调用栈中所有同步任务都执行完成后，将任务队列中的任务按顺序推出并执行。

异步任务又分成宏任务与微任务，任务队列中的任务都是宏任务，每个宏任务都会有一个自己的微任务队列。当宏任务执行完毕后，立马执行微任务队列中的任务。

宏任务：script标签代码，`setTimeout/setInteval`，`setImmediate`，`I/O(nodeJS)`，ajax请求，`postMessage`

微任务：`Promise`，`MutonObserver`，`Object.observe`，`process.nextTick`。

**三栏布局**：要求左右两边的盒子宽度固定，中间盒子宽度自适应，盒子高度随着内容撑高。通常中间盒子内容较多，为保证页面渲染快，**在写结构的时候将中间盒子放在左右盒子之前**。实现三栏布局的常用方法是**圣杯布局**与**双飞翼布局**。

圣杯布局实现方案：三个元素放在同一个父元素中，代表中间盒子的元素放在最前，父元素设置左右`padding`，三个盒子全部浮动——左边与中间设置左浮动，右边设置右浮动，设置中间盒子宽度 100%，左右盒子固定宽度，**设置左边盒子`margin-left: -100%`，设置相对定义，左边(`left`)平移自身宽度**。右边盒子设置右边距为自身盒子宽度，最后设置父元素清除浮动，否则父元素无法被撑开。

双飞翼布局实现方案：三个盒子对应三个元素，其中中间盒子套两层，中间盒子内部盒子设置`margin`(用来移除左边盒子对中间盒子的覆盖)。三个盒子全浮动，设置中间盒子宽度 100%，左右盒子固定宽度。左边盒子左边距-100%，右边盒子设置右边距-自身宽度，最后设置父元素清除浮动。(父元素右边需要有 padding，不然右边盒子会不见)

> 加分项：圣杯布局优点：无需添加额外的 dom 节点。缺点：当中间部分的宽度小于左边盒子时，会出现布局混乱。双飞翼布局优点：不会出现布局混乱。缺点：多添加一层 dom 节点。

XSS攻击通常分为反射型与存储型，这两者的前提都是有从用户那获取的数据动态的显示在网页上。

假设有网站A：`www.site.com?name=xxx`，会将从用户交互中获取的name显示在页面上，且没有对数据进行处理，则可以构造一个反射型XSS攻击。

```js
"www.site.com?name=<script>console.log(document.cookies)</script"
```

如果用户点击上面的链接，就会将A站点的cookie在控制台输出。

chrome浏览器线程数量：网络进程，GUI进程，渲染进程，插件进程，浏览器主进程。

浏览器渲染页面的过程中：对布局树进行分层的目的是避免一次性渲染整个页面，将整个内容分层不同的图层绘制，避免无用的渲染。光栅化的目的是：当页面很长，但可视区域较小时，则可以避免非可视区域的绘制。所以将每个图层又划分成多个小格子。

两个相邻的块级盒子之间设置margin会发生重合，原因是margin不是让盒子移动多少距离，而是元素旁边需要留多少空白。解决方法通常是父级盒子设置成BFC。

浏览器**强制缓存**在有效期内时，不会向服务器发送HTTP请求，如果是**协商缓存**，则会发送HTTP请求，若在有效期内，返回304，浏览器从本地获取数据，否则返回200，并返回新数据。

在创建ajax过程实际上就是使用xhr进行通信的过程，其中，`xhr.readyState`要为4时，才表示完成请求，一个会有5中状态。

- 0：表示初始化，已经创建xhr对象，但未执行open方法。

- 1：表示载入，已经调用open方法，但未发送数据

- 2：载入完成，已经发送请求

- 3：交互，可以接受部分数据

- 4：数据全部返回，`xhr.status`反映HTTP的响应码。

图片懒加载通常是修改`src`属性实现。默认情况下，将其设置为一个固定的加载图片，将真正的图片路径通过自定义属性`data-`存储，当元素到达可视区域时，才将真正的路径赋值给`src`属性。

# 排序

**冒泡**

```js
function sort(arr) {
  for (let i = 0; i < arr.length; i ++) {
    for (let j = 0; j < arr.length - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        let temp = arr[j]
        arr[j] = arr[j + 1]
        arr[j + 1] = temp
      }
    }
  }
}
```

**插入**

```js
function sort(arr) {
  for (let i = 0; i < arr.length; i ++) {
    for (j = i; j > 0; j--) {
      if (arr[j] < arr[j-1]) {
        let temp = arr[j]
        arr[j] = arr[j-1]
        arr[j-1] = arr[j]
      }
    }
  }
}
```

**快排**

```js
function quickSort(arr, low, high) {
  if (low < high) {
    let p = partion(arr, low, high)
    quickSort(arr, low, p - 1)
    quickSort(arr, p + 1, high)
  }
}
function partion(arr, low, high) {
  let p = arr[low]
  while(low < high) {
    while(low < high && arr[high] >= p) high--
    arr[low] = arr[high]
    while(low < high && arr[low] >= p) low ++
    arr[high] = arr[low]
  }
  arr[low] = p
  return low
}
```

**hooks不能够写在条件判断里**的原因是：react为所有hooks维护一个顺序表，hooks的返回值通过顺序索引获取，例如：

```js
const App = () => {
  if (true) {
    const [valueA, setValueA] = useState(0)
  }
  const [valueB, setValueB] = useState('0')
}
```

在内部，react维护的顺序表为`[0, '0']`，每次渲染时，顺序赋值。若某次if判断为假，则valueB拿到的值就会是0，而非字符串。

cookie的过期时间通过设置`expires`属性实现。每个站点设置cookie总数不能够超过20个。

HTTP中OPTION请求方法是用于**获取目的资源所支持的通信选项**。作用：1）监测服务器所支持的请求方法。2）CORS中的预检请求（preflight request）。

例如：通过发送OPTION请求，来获取服务器是否支持跨域请求，同时在预检请求返回中，服务器也可以通知客户端是否需要携带身份凭证。相关的HTTP头字段为

```json
{
  "Access-Control-Allow-Headers": '*'
}
```

告诉浏览器，实际请求时允许携带的字段。

session是设计的一种持久性网络协议，是一种比连接粒度更大的概念。一次会话可能包含多次连接，每次连接都被认为是会话的一种操作。主要是cookie存储在客户端，容易被篡改，而服务器无法识别是否被篡改。

由于session存储在服务器，数量过多，会带来压力。由此，产生token来验证用户，通过加密token来判断是否是服务器自己产生的token还是被篡改过。最流行的就是JWT。

HTTP1.0与HTTP1.1的区别：

1. 缓存处理
   HTTP1.0中主要使用请求头中`if-Modified-Since`，`Expires`来作为缓存判断标准，而HTTP1.1则引入更多的判断标识`if-Unmodifyed-Since`，`If-Match`等。

2. 带宽优化
   HTTP1.0中无法发送某个对象的部分信息，一旦请求，就会整个发送，且不支持断点传输。而HTTP1.1则可以只发送部分信息，通过`range`头部信息来控制。

3. 错误通知
   HTTP1.1中新增了24个错误状态响应码，如409，表示请求资源与当前资源状态相冲突，410，表示某个资源永久删除。

4. Host头处理
   HTTP1.0中认为一个IP对应一个主机，后续随着虚拟主机技术发展，一个IP可以对应多台主机，因此HTTP1.1中增加`Host`头域信息。

5. 长连接
   HTTP1.1支持长连接与流水线请求，在一个TCP连接上传输多个HTTP请求，默认开启`keep-alive`，而HTTP1.0在默认情况下，一个HTTP请求就会创建一个TCP连接，然后释放。

HTTP2.0最开始目的是为了减少页面加载延时，使用了图片压缩，多路复用等技术，后面以最初的技术模板与HTTP协议标准，整合了HTTP2.0。从技术角度来说，HTTP2.0与HTTP1.1最大的区别就是**二进制框架层(binary framing layer)**。

与HTTP1.1使用纯文本形式的请求与响应不同，HTTP2.0将所有的信息封装成二进制。但任然保留HTTP1.1的语法。

js的work有：`web worker`, `service worker`, `worklet`。都是与页面线程分开，在自己独立的线程上运行。

web worker：用来分流耗时长的脚本代码。

service worker：浏览器与网络或缓存之间的代理。同样在js中注册引用专门的处理文件，一旦启动，就能够拦截页面发出的所有请求。从而可以决策是从缓存中返回还是从网络中返回。可以实现离线web应用。

worklet：高度定制的worker，能够让开发人员参与浏览器渲染的各个阶段。

# 字节飞书一面

基本项目问题

- vim 模式自己实现?

- codemirror 光标为什么自己实现，（div是可以实现编辑能力的）

  > 最大的原因就是不同浏览器的`contentEditable`实现方式有所差异，在不同的文档内容中准确的定位光标很困难。

- vim的光标如何实现覆盖

  > 通过codemirror内置方法获取原始光标前的文字内容，随后定义一个额外的绝对定位的光标元素(`div`)，采用`::after`伪元素方法，将`content`属性设置为原始光标前的文字，这样就可以得到对不同字符实现不同的宽度。——实际上，即便不适用伪元素，直接使用一个绝对定位的div都是可以实现的，只要将光标前的文字添加正确。

- codemirror为什么部分加载

  > 根据用户手册，只使用部分加载是为了避免做一些事情，同时，也为可能是为了最小化浏览器的回流`reflow`。

- react diff算法

为了降低算法复杂度，react对diff算法进行三个限制。

1. 只对同级元素进行Diff。如果用一个DOM节点在两次更新中跨越了层级，则不会被复用

2. 两个不同类型的元素产生处不同的树，如果元素由`div`变成`p`则元素自身及子孙节点都会被重新创建

3. 可以通过`key`属性来明确指示哪些子元素在不同的渲染过程中是保持稳定的。

[diff 算法官网简介](https://zh-hans.legacy.reactjs.org/docs/reconciliation.html#the-diffing-algorithm)

- react fiber协调可中断，堆栈协调为什么不可中断

- react函数式组件与类式组件有什么不同

函数式组件或捕获当前渲染阶段的`props`与`state`。使得在当期阶段触发延迟事件后，立马更新渲染的情况下，函数式组件依旧是用旧值响应事件。

而类组件通常是将上面两个内容绑定到`this`上，方便获取最新的值，这就会产生一个常见错误，触发延迟事件时，立马更新渲染。如果事件中通过`this`获取内容，则会使用新的值来触发事件。

> 虽然类组件也可以显示的捕获，但在`render()`函数中写大量方法，则类的定义意义不大。

- 虚拟滚动

- 为什么React要用虚拟DOM

ChatGPT回答：

1. **性能优化**：通过批量更新的方法最小化对真实DOM的操作，可以减小重排与重绘次数。提高性能

2. **跨平台兼容性**：虚拟DOM不依赖浏览器环境，可以在服务端渲染，也可以在移动端渲染

3. **开发效率**：简化对真实DOM的操作，让开发者更专注于数据与业务逻辑，而无需手动操作DOM，提供一种声明式编程模式，将页面状态与渲染逻辑分离

**实际上使用虚拟DOM是有额外开销的**，但是，网上一种说法是在没有使用虚拟DOM的情况下，无法准确定位页面更新的内容。没办法才加上一层虚拟DOM的逻辑。

> 虚拟DOM不是一项功能，是达到目的的一种手段——声明式，状态驱动UI，让开发者在构建应用时无需考虑状态的装换——[svelte](https://svelte.dev/blog/virtual-dom-is-pure-overhead)

- 编程一：考察`setState`更新方式，要替换。组件复用问题——dom结构要变？

 > 回答的是编写一个自定义钩子，不过应该是想考察上下文，通过`createContext`构建一个上下文组件。

```jsx
const Context = React.createContext()
export function useCustomContenxt() {
  return useContent(Context)
}
export function CustomProvider({childern}) {
  return (
    <Context.Provider
      value={}
      >
      {children}
    </Context.Provider>
  )
}
```

- 编程二：输出被括号包裹的内容`(a+b)*c(a+(d+f)-c)`。

> 当时卡在嵌套括号时，需要用到已经出栈的数据，实际上将数据重新压入就好。

```js
function getContent(str) {
  const stack = []
  const res = []
  for (let i = 0; i < str.length;) {
    if (str[i] === '(' || stack.length > 0) {
      while (i < str.length && str[i] !== ')') {
        stack.push(str[i])
        i += 1
      }
      let subStr = ''
      while (stack.length > 0) {
        let temp = stack.pop()
        if (temp !== '(') {
          subStr = temp + subStr
        } else {
          break
        }
      }
      if (stack.length > 0) {
        stack.push(`(${subStr})`)
      }
      res.push(subStr)
    }
    i += 1
  }
  return res
}
```