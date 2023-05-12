# 面经

### js

**promise**是什么以及使用方法：将三种状态，promise 对象方法，微任务（Promise.resolve().then()）是一个微任务，会在宏任务后面立即执行。

什么是跨域，如何解决：同源限制，协议，域名，端口，CORS，node 中间件，JSONP，postmessage。  
--- node 中间件，nginx 反向代理，就是通过代理进行请求发送，前端与代理服务器之间是同源的，然后，服务器之间是不存在同源限制的。  
--- JSONP 的原理是，跨域问题是请求文件产生的，但请求`script`是不会产生跨域问题。同时设置响应头文档类型为`javascript`。因为脚本标签本来就是能够引用网络上的 JS 文件的。使用的是 GET 请求。在不适用 JSONP 的时候，该标签是通过请求拿到脚本文件直接执行，若使用的是 JSONP，则返回的是客户端传过去的回调函数与文件内容（作为回调参数），然后在本地执行回调函数。  
--- postmessage 则是 H5 的新端口，通过发送与接受 API 实现同信。

JS 中判断变量类型：一共四种方法，typeof，instanceof，Object.prototype.toString.call(obj)对象原型链，constructor 引用数据类型，就是判断实例的构造函数与类的构造函数是否相同，避免了原型链干扰。最精准的就是第三种。

JS 中实现异步的方式：回调函数，事件监听，setTimeout，Promise，生成器，async/awati。回调函数——Ajax 回调。生成器 ES6 语法，因为该函数不是一次执行完，可以通过 yield 保存不同阶段状态，通过 next 返回。

数组去重方法：通过对象属性，就是遍历数组的时候，将数据作为零时对象的属性，若没有，则添加属性，若有，则抛弃数组值，Set 有兼容性问题，filter，reduce+includes 等，filter+indexOf，原理是后者只返回第一个匹配到的数据下标。

undefined 与 null：undefined 是全局对象的一个属性，声明的变量为赋值，没有返回值的函数返回值。函数形参没有赋值，undefined==undefined，undefined===undefined，访问对象中的不存在属性。null 代表**对象**的值未设置，相当没有设置指针地址。**null 的 typeof 是 object**。null 是人为设置的值。可以将对象赋值为 null 来释放对象。

ES6 中的箭头函数：没有 this，从外部获取，没有 arguments，不能用 new，没有原型与 super。  
--- 通过`call`与`apply`调用时，第一个参数会被忽略，即无法绑定 this。无法使用`yield`关键字，因此箭头函数也不能够作为生成器函数。

说一说 HTML 语义化：语义化标签，利于页面内容结构化，利于无 CSS 页面可读，利于 SEO，利于代码可读。  
--- 易于用户阅读，样式文件为加载时，页面结构清晰。  
--- 易于 SEO，搜索引擎根据标签来确定上下文与关键字权重  
--- 方便屏幕阅读器解析，例如盲人阅读器根据语义渲染网页  
--- 利于开发和维护，代码可读性高

this 指向问题：全局执行上下文、函数执行上下文、this 严格模式下 undefined、非严格模式 window、构造函数新对象本身、普通函数不继承 this、箭头函数无 this、可继承。  
--- 关键字由来，在对象内部的函数中使用对象内部的属性是非常普遍的需求，但 JS 作用域机制无法实现，就创造了 this 机制。  
--- 从全局执行上下文，函数执行上下文。

JS 变量提升：注意函数提升是优先于变量，let，const 会有暂时性死区，初始化之前访问会报错。

HashRouter 与 HistoryRouter 区别：二者都是通过浏览器的特性实现的。1）history 是通过浏览器的历史记录栈 API 实现。hash 则是监听 location 对象的 hash 值变化事件实现。2）url 差别。3）同一个 url 刷新，history 会触发浏览器历史记录栈，而 hash 不会。4）history 需要后端配置指向 index.html，否则刷新会出现 404 问题。hash 不需要。

原理：HashRouter 通过`windown.onhashchange`方法获取新 URL 中的 hash 值。HistoryRouter 则是通过`history.pushState`来实现页面跳转是不触发页面刷新，通过`window.onpopstate`监听前进与后退。

map 与 forEach 的差别：除了自己知道的，map 处理速度是比 forEach 快的，同时方便使用链式调用其他数组方法。

**Event loop，宏任务，微任务**

得分点：任务挂起，同步任务执行结束，执行队列中异步任务，执行`script`标签内代码，`setTimeout/setInterval`，`ajax`，`postMessage/MessageChannel`，`setImmediate`，`I/O(nodejs)`，`Promise`，`MutationObserver`，`Object.oberve`,`process.nextTick(nodejs)`，每个宏任务中，都包含一个微任务队列。

---

**_浏览器的事件循环机制_**：执行 JS 代码时，遇见同步任务，就直接推入**调用栈**中执行，遇到异步任务，将该任务挂起，等到异步任务有返回之后，推入到队列之中，当**调用栈**中所有同步任务执行完毕，再将任务队列中的任务按顺序推入并执行。如此循环。

**_异步任务_**：异步任务又分为宏任务与微任务。

**宏任务**：任务队列中的任务，称为宏任务，**每个宏任务中都包含一个微任务队列**。

**微任务**：等宏任务中的主要功能都完成后，渲染引擎不着急去执行下一个宏任务，而是执行当前宏任务中的微任务。

宏任务包含：执行`script`标签内部代码，`setTimeout/setInterval`，`ajax`，`postMessage/MessegeChannel`，`setImmediate`，`I/O(nodejs)`，DOM 事件。

微任务包含：`Promise`，`MutationObserver`（监视 dom 元素的改变），`Object.observe`（监视某个对象属性的改变），`process.nextTick(nodejs)`。

> 浏览器与 node 环境中，微任务的执行时间不同，node 端中，微任务在事件循环的各个阶段之间执行，而浏览器中是在宏任务执行完后进行。

> 在分析事件循环过程中，**一整块代码就是一个宏任务**，因此，分析过程应该是：同步任务，当前文件中的微任务，文件中记录的宏任务。

---

### css

什么是 BFC：块级格式化上下文、独立渲染区域、不会影响边界以外的元素。产生 BFC 的条件：float，position，overflow，display。

--- 使用 BFC 的目的是创建独立的渲染区域，内部的元素渲染不会影响外界。常见的应用场景是清除浮动（就是使用了`float`，属性是让元素沿着其容器的左侧或右侧放置，能够让文本与内联元素环绕它，会部分脱离文档流，也就是元素本身大小不会去撑开父容器。）。在前面的描述中，由于浮动元素脱离文档流，其父容器的渲染就会影响到外部的元素（因为浮动元素不会把父元素撑开）。为了让浮动元素撑开父容器——盒子内部的东西不会跑到外部。将父容器设置为 BFC 就可以了。但有因为浮动是部分脱离，所有，不会有兄弟元素跑到浮动元素的后背，被遮住。  
--- 产生方式，1）overflow 非 visible 赋值（最常用为 hidden）。2）float 设置左或右。3）position 为绝对或固定。4）display 设置为 flex，inline-block 等。  
--- 其他相关的内容，IFC（inline formatting context）内联格式化上下文，GFC（grid formatting context）网格布局格式化上下文，display 设置 grid，FFC（Flex formatting context）自适应格式化上下文，display 设置 flex 或 inline-flex。

伪类与伪元素：自己查找的  
--- 伪类：一种选择器，用于选择处于特定状态的元素，比如鼠标悬浮在某个元素上，某个类型中第一个元素等，就像对文档的某个部分应用了一个类一样。通常接在`:`后面。表现就像为特定部分添加了类。  
--- 伪元素：类似伪类，就像往文档中添加了新的 HTML 元素一样，而不是向现有元素上应用类。伪元素通常接在`::`后。例如一个`p`元素中有长文字，那么想让在屏幕上第一行变粗，其他不变，可以在文字上添加新标签，但当屏幕大小改变，则有需要重新调整新标签位置，而伪元素`::first-line`则会一直指向屏幕上第一行的内容，即便动态改变屏幕大小。

浮动：脱离文档流，盒子塌陷，影响其他元素排版，伪元素，`overflow:hidden`，标签插入法。  
--- 设置浮动元素，块级元素可以在同一行，行内元素可以设置宽高——就是自动装换成行内块。  
--- 通过伪元素清除浮动方式，与在父类添加 overflow 是一样的，`.clear-float::after{content:''; display: table; clear: both}`，前面是牛客上的写法，实际上，table 就是创建一个块级元素，clear 的意思是是否让元素移动到他之前的浮动元素下面，content 的内容就是伪元素显示的内容，这里为空。

css 尺寸设计问题：px，rem，em，vw，vh。注意的是，em 用在其他属性上，是相对于自身字体大小。在移动端的响应式设计中，通过 rem 设置字体，然后，配合不同屏幕尺寸在根节点字体大小不同，展示的内容的字体大小也会相应改变。

**元素水平垂直居中**

- 父元素相对定位，目标元素绝对定位，设置 top，left 为 50%，同时移动自动大小的一半`transform: translate(-50%, -50%)`，兼容性最好

- 父元素设置为 flex，同时设置`justify-content: center; align-items: center`，目前也是主流

- 父元素设置为 grid，同时设置`justify-content: center; align-items: center`，同 flex

- 父元素设置为`display: table-cell`，同时设置目标元素为`text-align: center; vertical-align: middle`

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

浏览器的渲染过程

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

- header：记录本次 JWT 使用的加密算法，类型等，基本是不变的（常用的是 HS256 算法），是一个对象，采用 base64 编码

- playload：一些用户信息（不要放敏感信息），采用 base64 编码

- Signature：前面两部分通过秘钥加密后的结果，同样采用了 base64 编码

> 与 session 的区别：session 是存储在服务器中，JWT 存储在客户端中。

#### 浏览器的缓存机制

> 浏览器可以根据相应报文中的缓存标识决定是否将内容缓存。是则将请求结果与缓存标识存入浏览器。

- 每次请求前，在缓存中查找是否请求结果与缓存标识

- 没有则发送新的请求，每次得到新的请求结果，都将结果与缓存标识存入缓存中。
