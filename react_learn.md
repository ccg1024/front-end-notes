# ReactJS

> 记录React学习过程中的一些难点

React特点

1. 采用**组件化**模式、**声明式编码**，提高效率与组件复用率。
2. 使用**虚拟DOM**+优秀的**Diffing**算法，尽量减少与真实DOM的交互。

**虚拟DOM**（就是一个js对象）

在拿到数据时，React先将数据声成虚拟DOM，并与原本的虚拟DOM进行比较，与原本相同的数据就不会重新生成**真实DOM**，只会将原本没有的数据生成真实DOM。

> 在jsx中写的html节点就是虚拟DOM，通常原生的js是没法直接写虚拟DOM的，但 自己的项目中也写了，应该是后台默认转化了，如果在html文件中写js脚本是无法这么写的。

**babel.js**: 一个转化文件，能够就将ES6规范转化成ES5。也能够将jsx转化成js，浏览器只认js。

> XML主要是早期传送数据用的格式，现在主流应该是JSON.

**!!!在虚拟DOM中，大括号(`{}`)内只能写JS表达式，这也就是为什么在写自己项目的时候没法用if语句的原因了**。

> JS表达式，通常是那些写在等号右边的代码。

**Diffing算法**

该算法依赖组件上的`key`属性，每个`key`属性认为是独立的。类似`id`。这也是官网上提到过可以通过修改组件的`key`来重新渲染该组件。

### 组件

定义：代码与资源的结合。通常使用了`state`的组件为复杂组件，没有使用的为简单组件。

#### 函数组件

适用于定义简单组件。

#### 类组件

适用与定义复杂组件。

#### 三大核心属性

> 默认是通过**类**定义的属性。React18中`useState`是属于在函数组件中实现类式组件的功能。

----

1. **state**

用于存储显示在页面上的数据。状态组件写法

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props)
    this.state = {} // initial state, should be a object
  }
  render() {}
}
```

函数组件中，事件`this`的问题，由于React中事件监听的回调函数是以赋值的方式传递，最终调用类中函数的对象不再是实例化对象，所以需要创建绑定好`this`对象的函数。

```javascriptreact
class Weather extends React.Component{
  constructor(props) {
    super(props)
    this.state = {isHote: false}
    // 加上这一句就可以解决下面注释说的问题
    this.changeWeather = this.changeWeather.bind(this)
  }
  render() {
    const {isHot} = this.state
    return <h1 onClick={this.changeWeather}>hot {isHot ? 'yea' : 'no'}
  }
  changeWeather() {
    // 由于事件绑定是以函数赋值方式传递，所以触发事件时this会丢失，不会指向Weather对象。
    console.log(this)
    this.setState({// 同名替换，其他不变
      isHote: !this.state.isHote
    })
  }
}
```
> 类中的方法默认都是严格模式

通常，如果只是初始化状态，可以不用写构造函数。如下为精简版

```javascriptreact
class Weather extends React.Component{
  state = {isHote: true}
  render() {
    const {isHot} = this.state
    return <h1 onClick={this.changeWeather}>hot {isHot ? 'yea' : 'no'}
  }
  changeWeather = () => {
    this.setState({
      isHote: !this.state.isHote
    })
  }
}
```

精简版中，`state`直接写成成员变量，`changeWeather`也以成员变量的形式给出，这样该函数就不是在类的原型对象上，而是直接在实例对象上。同时使用箭头函数，当直接调用该函数时，会继承外面的`this`，也就是实例化对象。

* * * *

2. **props**

从组件外传入数据给组件内部。在组件内部为只读属性。

```javascriptreact
class Person extends React.Component{
  render() {
    this.props.name // ada
  }
}
// 下面两个可以放到类中的静态属性上
// 对于属性的类型进行限制，需要PropTypes包
Person.propTypes = {
  name = PropTypes.string.isRequired // 必传字符串
}
// 属性默认值
Person.defaultProps = {
  sex: 'man'
}
const obj = {name: "1", second: 2}
<Person name="ada" />
<Person {...obj} />  // 批量传递
```

> 再一次，类式组件的构造器能不写就不写。官网的描述也是仅用于`state`初始化，与设置绑定`this`的事件回调函数

* * * *

3. **ref**

效果同`useRef`，就是对某个Dom元素的引用。在中使用方法为

```javascriptreact
class Demo extends React.Component {
  thirdTag = createRef()
  showDate = () => {
    console.log(this.refs.myName)
  }
  render() {
    return (
      <div onClick={this.showDate}>
        <div ref="myName"></div>
        <div ref={(tag)=>{this.myTag = tag}}></div>
        <div ref={this.thirdTag}></div>
      </div>
    )
  }
}
```

按照官网的建议，字符形式的`ref`最好是不要在使用，在某些情况下会出现性能问题。并且后续版本可能会删除。最好采用回调形式的`ref`。

回调形式的`ref`执行次数：如果回调函数以内联的形式写在标签上，那么在更新组件的阶段(第一次渲染不会)会调用两次内联回调函数。第一次为`null`。第二次为具体的标签。可以将回调函数写在类里面就可以只调用一次。或者使用最新的函数`createRef`。

> 但是到了React18开始，应该是推荐使用函数式组件。但还会尽量少用`ref`。毕竟要大量存储Dom内容。

### 收集表单数据

包含表单的组件分类：

1. 受控组件：通过`onChange`事件获取数据，并利用回调函数将数据存储到`state`中
2. 非受控组件：表单中所有输入类组件，数据现用现取。通过`ref`获取数据，数据本身没有存储

```javascriptreact
// 非受控组件表单
return (
  <form onSubmit={this.handleCallback}>
    <input ref={this.myRef} type="text" name="user" />
  </form>
)

// 受控组件表单
return (
  <form onSubmit={this.handleCallback}>
    <input onChange={this.handleChange} type="text" name="user" />
  </form>
)
```

> 通常情况下，能够写受控组件就写受控组件(类似Vue中的双向绑定)，能够避免使用大量的`ref`。

#### 函数柯里化/高阶函数

在受控组件中，每个输入项都会接受一个回调函数，用于将数据存入状态中，若每个组件都写一个函数，那么就会出现大量相似的函数。通常将这些函数抽象成一个总体函数，通过参数来控制存储不同的状态信息。


```javascriptreact
class Demo extends React.Component {
  saveFormData = (dataType) => {
    return (event) => {
      // data value
      // 不能直接写dataType,这样会默认把他当成字符常
      // 需要读取变量
      this.setState({[dataType]:event.target.value})
    }
  }
  render() {
    return (
      <form>
        <input onChange={this.saveFormData('firstInput')} />
      </form>
    )
  }
}
```

> **！！！注意**，JS的对象写法中，`key`的值都是默认转成字符串的，若需要读取变量的值需要添加中括号。

**高阶函数**：若函数接收的参数为函数，或者返回值是一个函数，则该函数为高阶函数。

**函数柯里化**：通过函数调用继续返回函数的方式，实现多次接受参数，最后统一处理的函数编码形式。

_上述案例就应用里高阶函数与函数的柯里化_。

* * * *

### 组件的生命周期

就是之前在老版的官网看到的三个成员函数：`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`。同样的函数式组件使用的`useEffect`相当于实现上面三个方法的功能。

> 每次渲染都是调用`render()`方法。但前最新的官网介绍中，前三个方法都是过时方法。

旧版React的生命周期图（React16版）

![生命周期-旧](./img/react生命周期-旧.png "旧版生命周期")

旧版图中，左侧为第一次加载组件，右侧为组件更新，分三条线：（1）通过设置状态更新。（2）通过强制更新函数强制更新。（3）父组件更新，出发自身更新。

> `shouldComponentUpdate`返回一个布尔值，为真则触发后续更新，为假则阻断更新。

新版React生命周期（React17版）

![生命周期-新](./img/react生命周期-新.png "新版生命周期")

在新版本的React中，计划弃用三个常被误用的钩子函数。同时提出了两个新的钩子。

- `componentWillMount`
- `componentWillUpdate`
- `componentWillReceiveProps`

新增的两个钩子（实际上React18推荐函数组件，新增的也过时了。即与类组件相关的都算过时）。

- `getDerivedStateFromProps`：静态方法，返回值用于更新`state`值。通常只用于非常罕见的案例——`state`的值依赖于`props`中的值。
- `getSnapshotBeforeUpdate`：更新前获取快照，返回一个`null`或快照值（任意数值类型）。返回值将传递给`componentDidUpdate`。某些UI场景可能需要使用。


### Diffing算法

状态更新，导致重新调用`render`进行渲染时，并不会把整个组件都重新在网页上进行替换，而是对比数据，只将数据变动的部分修改。最小单位为一个标签节点。

那么对比过程中，属性`key`就比较关键了，也是经典的面试题目。通常的问法。

1. react/vue中的key有什么作用？（key的内部原理是什么）
2. 为什么遍历列表时，key最好不要用index？

* * * *

**虚拟DOM中key的作用**：

简单说：key是虚拟DOM对象的标识，在更新显示时key起着重要的作用。

详细说：当状态中的数据发生变化时，React会根据「新数据」生成「新的虚拟DOM」，随后React进行「新虚拟DOM」与「旧虚拟DOM」的diff比较，比较规则如下：

1. 「旧虚拟DOM」中找到了与「新虚拟DOM」相同的key

   - 若「虚拟DOM」中内容没变，直接使用之前的真实DOM
   - 若「虚拟DOM」中的内容变了，则生成新的真实DOM,并替换页面

2. 「旧虚拟DOM」中未找到与「新虚拟DOM」相同的key，则根据数据创建新的真实DOM，并渲染页面

* * * *

**用index作为key可能引发的问题**

1. 对数据进行逆序添加、逆序删除等破坏顺序操作，会产生没有必要的真实DOM更新（界面效果没问题，但效率低）

2. 如果结构中还包含输入类DOM：会产生错误DOM更新（界面有问题）

3. **注意**！如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，仅用于渲染列表，使用index作为key是没有问题的

```html
<!-- old virtual dom -->
<li key=0>小李<input type="text"/></li>
<li key=1>小张<input type="text"/></li>

<!-- new virtual dom -->
<li key=0>小王<input type="text"/></li>
<li key=1>小李<input type="text"/></li>
<li key=2>小张<input type="text"/></li>
```

上面的代码中，逆序添加了一条数据，将导致三条数据都重新渲染，但是，Diff算法比较内部的`<input/>`标签时发现没有改变，就会沿用上一次的内容，而这将导致原本小李的输入框会放到小王身上，这样输入框的内容就与前面的名字不对应了。

* * * *

**开发中如何选择key**

1. 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证、学号等

2. 如果确定只是简单的展示数据，也是可以用index的。

### 脚手架

`create-react-app`创建的项目通常由React, webpack, es6, eslint构成。

