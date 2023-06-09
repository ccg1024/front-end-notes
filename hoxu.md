### 宏任务，微任务

在 JavaScript 中，宏任务和微任务是指在执行代码的过程中的两种不同的任务类型。

**宏任务**（macro task）指的是浏览器在执行代码的过程中会调度的任务，比如事件循环中的每一次迭代、setTimeout 和 setInterval 等。宏任务会在浏览器完成当前同步任务之后执行。

**微任务**（micro task）指的是在当前宏任务执行完成之后立即执行的任务，比如 Promise 的回调函数、process.nextTick 等。

在 JavaScript 中，宏任务和微任务是相互独立的，在一次事件循环中会先执行所有的宏任务，然后再执行所有的微任务。

**宏任务会在同步任务执行完之后执行，而微任务会在当前宏任务执行完之后立即执行**。
