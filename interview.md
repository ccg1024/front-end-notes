---
title: 面试总结
desc: 记录实际面试中提到的问题
---
# Archlinux 记录

## fiber

react16 提出的新的协调算法，旧版本的协调算法为`virtualDOM`，内部使用的是堆栈方式（也称为堆栈协调器）进行协调，一旦开始就不可打断。

Fiber 是针对旧协调器编写的**向后兼容的**的版本。协调算法称为`Fiber Reconciler`。常用来表示 DOM 书的节点。

> Fiber 树可对应一个 DOM 树。

Fiber 协调器主要是提供增量渲染，将整个渲染过程拆分成多个时间帧。能够为每个工作单元定义优先级的能力，暂停，重复使用，中止工作的能力。

在计算新的渲染更新时，React 会多次访问主线程。让高优先级的工作可以跳过低优先级的工作。

### 术语

#### 协调（reconcile）

协调是两个 DOM 树的 diff 算法，当用户界面第一次渲染时，React 创建一个节点树。每个单独的节点都是一个 React 元素（对象）。

react diff 算法是保持两种假设：1）不同类型的元素产生的是不同的树。2）可以通过`key`属性来控制哪些元素在重新渲染过程中保持不变。

#### 调度

基于时间或优先级来安排不同的渲染工作。

#### requestIdleCallback（请求闲置回调）

`requestAnimationFrame`安排高优先级的函数在下一个动画帧之前被调用。`requestIdleCallback`安排低优先级或非必要的函数在帧结束的空闲时间被调用。

#### Fiber 结构

fiber 是一个普通的 js 对象，代表一个 React 元素或 DOM 节点。作为一个工作单位。当 react 第一次渲染的时候，就会创建一个 fiber 树。虽然是树结构，但内部有链表指向不同的元素，例如：每个 fiber 都将使用`retuer`键来指向其父元素，表示完成工作后要返回到父节点。更准确的说是一个列表树。

遍历 fiber 树进行更新过程中，存在两颗树：`current tree`，`workInProcess tree`。遍历 fiber 树的过程采用深度优先原则，从根节点开始，优先遍历子节点，直至叶子节点，随后遍历兄弟节点及其子节点。没有兄弟节点则返回父节点。

在初次渲染阶段，会为每个 react 元素创建一个 fiber 节点，后续更新阶段中，不会为每个 react 元素创建新的节点，而是将新数据与原来的 fiber 节点进行合并。

在 react15 之前，堆栈协调器是同步的，所以，每次更新都会递归的遍历整个树，并制作树的副本，如果中途出现优先级更高的更新，无法停止当前更新并执行优先级更高的更新。

Fiber 将更新划分成工作单元，通过`requestAnimationFrame`来处理优先级高的更新，通过`requestIdleCallback`来处理优先级低的更新（内部有`deadline`来控制时间）。在调度期间，Fiber 检查当前更新的优先级和`deadline`（帧结束后的自由时间）。

如果优先级高于待处理事件，或者`deadline`没有或尚未到达，Fiber 可以在一帧之后安排多个工作单位元。而下一组工作单元会被带到更多的帧上。Fiber 可以中止，暂停，重用工作单元的原因。

#### 工作内容

通过两个阶段完成工作内容，`render`与`commit`。

##### 渲染阶段

树形遍历与`deadline`的使用在次阶段，用户不可见。该阶段也称为协调，从根节点开始，为每个 fiber 调用`workLoop`函数执行工作。过程分为`begin`与`complete`

**begin**

在`workLoop`函数中，通过`nextUnitOfWork`判断是否还有需要工作的单元，有则将其传递给`performUnitOfWork`函数，该函数内部调用`beginWork`函数，而`beginWork`函数判断该 fiber 是否有待处理的工作，没有则直接跳过，有则根不同的 fiber 类型调用不同的更新方法。如果有子 fiber,`beginWork`会返回该 fiber,否则返回空。随后，通过`performUnitOfWork`来递归遍历子 fiber。遍历到最后，`performUnitOfWork`函数将调用`completeUnitOfWokr`函数。进入完善阶段。

**complete**

`completeUnitOfWork`会调用`completeWork`函数来完成实际的工作，同`performUnitOfWork`函数一样，`completeUnitOfWork`也只是一个循环迭代的地方。如果存在，`completeUnitOfWork`会返回一个同级的 fiber 执行下一个工作单元，否则返回父 fiber。

##### 提交阶段

该阶段是用户可见的渲染阶段，因此是同步渲染，无法无法拆分成部分渲染。该阶段 Fiber 已经有 UI 上渲染的 current 树，finishedWork，在渲染阶段建立的 workInProgress 树，效果列表。效果列表（effect list）是 workInProgree 树的子集，通过 nextEffect 指针进行链接，实际的 DOM 更新是发生在效果列表上的。该阶段 workInProgree 树将成为新的 current 树。

# 阅文

cookie 大小，4k，如果设置超过 4k 的内容，就会被裁剪。

在一个页面中通过`windows.open`方法打开新页面，新页面中会存在旧页面中`sessionStorage`设置的内容，但是两个页面中的`sessionStorage`是不共享的。

`localStorage`跨域问题。**只要不同源就不共享数据**。

React 中 Fiber 原理。渲染过程中使用到的**两个函数**

事件循环，宏任务与微任务。

react 中与 vue 的双向绑定

跨域问题：`webpack`的代理只能够是本地的，无法应用在服务器上。

跨域解决方案：jsonp，cors，nginx 代理，nodejs 中间件，`document.domain+iframe`，`location.hash+iframe`，`window.name+ifrmae`，`postMessage`，`webSocket`。
