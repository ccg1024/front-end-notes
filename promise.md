# Promise

> ES6引入的处理异步操作的类，提供让异步编程像同步编程一样的编程模式。

手写的`Promise`实例方法如下。

```javascript
let p = new Promise((resolve, reject) => {
  // some code
  if (condition) {
    resolve()
  } else {
    reject()
  }
})
p.then(() => {
  // if run resolve
}, () => {
  // if run reject
})
```
上面的代码第一块是运行的构造函数，第二块调用实例方法。通常情况下`Promise`是一个函数的返回对象。通常，`resolve`方法会将`p`设置成fulfilled状态，`reject`会设置成rejected状态。

在MDN网站上对于该对象的代码描述为

```javascript
// 成功的回调函数
function successCallback(result) {
  console.log("音频文件创建成功：" + result);
}
// 失败的回调函数
function failureCallback(error) {
  console.log("音频文件创建失败：" + error);
}
createAudioFileAsync(audioSettings, successCallback, failureCallback)

// using promise
const promise = createAudioFileAsync(audioSettings);
promise.then(successCallback, failureCallback);
```

相较于传统的写法，在异步方法调用前将成功与失败的回调函数传入该异步方法，使用`Promise`的写法将是异步函数返回一个对象，通过该对象绑定函数的成功回调与失败回调。

使用`Promise`时，存在一下约定

- 在本轮 事件循环 运行完成之前，回调函数是不会被调用的
- 即使异步操作已经完成（成功或失败），在这之后通过`then()`添加的回调函数也会被调用，因为then是绑定回调对象，通常来说，事件触发时有绑定对象才会调用，没有就没有调用，但这里是即便已经触发完了，后面绑定的也会执行。
- 通过多次调用`then()`可以添加多个回调函数，它们会按照插入顺序进行执行

`catch(failureCallback)`是`then(null, failureCallback)`的缩略形式。

**Promise三种状态**

1. 待定（pending）：初始状态，既没有兑现，也没有拒绝
2. 已兑现（fulfilled）：异步操作以及完成
3. 已拒绝（rejected）：异步操作失败

> 实际上`Promise`就是然异步操作相同步一样，因为异步操作不会立马有返回值，通过定义返回值为`Promise`，保证在未来的某个阶段把值交付，就像单词本意一样。承诺

![Promise](./img/promises.png)

### 回调地狱

传统异步编程采用回调函数实现，若异步操作之中嵌套异步操作，通常就称为回调地狱。如下

```javascript
asyncfun1(param1 => {
  // first callback
  asyncfun2((param1,param2) => {
    // second callback
    asyncfun3((param1,param2,param3)=>{
      console.log(param1, param2, param3)
    },failureCallback)
  },failureCallback)
},failureCallback)
```

### 常见情况

通常应用在`AJAX`请求方法上。

### 对象方法

`resolve`与`reject`两个方法都可以接受一个参数，用于描述当前被改变状态的`Promise`的结果信息。

```javascript
let p = new Promise((resolve, reject) => {
  resolve('some result info')
})
p.then((value)=>{
  console.log(value) // som result info
})
```

同异常捕获一样，无论状态是兑现还是拒绝，`finally()`中的回调函数都会执行。通常放置一些无论异步操作是否成功都需要执行的操作。

> 目前为止，所有的Promise方法都会返回一个`Promise`对象。即便是`finally`方法。

在`then`方法中，可以通过`return`将当前`Promise`状态修改成已兑现。

```javascript
const p = new Promise((resolve) => {
  resolve()
})
const t = p.then(() => {
  return 123
})
t.then((value) => {
  console.log(value) // 123
})
```

> **注意，只要代码体中产生错误（手动抛出也算），返回的`Promise`都会是已拒绝状态。

### async/await

是ES2017(ES8)基于`Promise`提出的解决异步的最终方案。通过`async`修饰的函数会默认返回一个`Promise`对象`resolve`的值。

```javascript
async function fn() {
  return 1
}
// equal to 
function fn() {
  return Promise.resolve(1)
}
```

`await`必须在`async`修饰的函数内部使用，就是等待的字面意思，当后面接的是`Promise`对象时，将等待该异步操作完成，并且获取到`resolve`/`reject`的值。若为非`Promise`对象，则按原本的代码返回。

```javascript
async function fn() {
  let a = await Promise.resolve(1) // wait for finish
  let b = await 'string'
  console.log(a, b) // 1, string
}
```
