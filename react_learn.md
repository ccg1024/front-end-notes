# ReactJS

> 记录React学习过程中的一些难点

React特点

1. 采用**组件化**模式、**声明式编码**，提高效率与组件复用率。
2. 使用**虚拟DOM**+优秀的**Diffing**算法，尽量减少与真实DOM的交互。

**虚拟DOM**（就是一个js对象）

在拿到数据时，React先将数据声成虚拟DOM，并与原本的虚拟DOM进行比较，与原本相同的数据就不会重新生成**真实DOM**，只会将原本没有的数据生成真实DOM。

> 在jsx中写的html节点就是虚拟DOM，通常原生的js是没法直接写虚拟DOM的，但自己的项目中也写了，应该是后台默认转化了，如果在html文件中写js脚本是无法这么写的。

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

----

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

----

3. **ref**

效果同`useRef`，就是对某个Dom元素的引用。在中使用方法为

```javascriptreact
class Demo extends React.Component {
  showDate = () => {
    console.log(this.refs.myName)
  }
  render() {
    return (
      <div onClick={this.showDate}>
        <div ref="myName"></div>
        <div ref={(tag)=>{this.myTag = tag}}></div>
      </div>
    )
  }
}
```