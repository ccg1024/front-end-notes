# webpack

> 简单入门`webpack`。目前前端主流的工程化解决方案之一。

项目中安装`webpack`与`webpack-cli`，同时，`npm`命令添加`-D`就是`--save-dev`，

- `-D`表示在`devDependencies`进行添加，表示只在开发阶段才会使用

- `-S`相当于`--save`写入到`dependencies`对象，表示开发环境和生产都使用

安装固定版本的WebPack.

```shell
npm install webpack@5.42 webpack-cli@4.9.0 -D
```

> 快速建立npm项目的命令，`npm init -y`。

### 启动命令

```json
{
  "scripts": {
    "dev": "webpack",
  }
}
```

### 基本使用

**webpack.config.js**真正开始打包之前读取的配置文件信息。

```js
const path = require('path')
module.exports = {
  mode: 'devlepment' // 模式，开发/生产
  entry: path.join(__dirname, './src/index.js'), // 打包入口
  output: {
    path: path.join(__dirname, './dist'), // 输出文件路径
    filename: 'js/bundle.js', //把生成的bundle.js放在dist/js/bundle.js下面
  }, // 打包出口
  
}
```

> 若要自动进行打包可以使用`webpack serve`启动`webpack-dev-server`应用。

在`package.json`文件中添加新命令，方便启动打包服务。

```json
{
  "scripts" {
    "serve": "webpack serve"
  }
}
```

同时在`webpack.config.js`中可以配置服务器的一些信息，例如端口什么的。

> 就是在本地拉起一个服务器，方便看到程序执行时的样子。

**bundle.js**使用来将文件中引用的依赖自动打包的js文件。`webpack`的工作流程算是围绕这个文件展开的。

### B站视频

处理图片内容使用的`loader`是`file-loader`与`url-loader`，其中，前者是对文件不进行修改。原封不动的输出。后者则是对小与某个大小的图片输出base64格式。已经内置到webpack5。只需要配置激活。

**css**文件默认是打包到js文件中的，但实际生产环境会出现闪屏现象，（先加载html, js, 通过js插入style的css）。因此需要使用额外的插件将css提取到单独文件，通过link引入。

webpack开发过程中优化：

- source-map：浏览器默认提示错误是编译后的文件，不方便定义源码在那，该插件能够将源码与打包后的文件建立映射，生成map文件，浏览器会自动解析，提示源码错误位置

- HMR：热模块替换功能，每次重新打包只会处理修改部分，但默认不支持js，不过react这些框架有自己的插件/loader能够处理。

- one-of：每个文件都只会使用一个loader，但配置上会有很多loader去匹配，因此，将loader配置在该属性里，生效一个loader后就会停止匹配。

- include/exclude：如名，只能配置一个，打包时包括或剔除某些文件

- eslint/babel：缓存，第一次打包缓存所有，后续只会对修改文件进行处理

- 多进程打包：通过插件开启多个进程的打包

- tree-shaking：默认对于第三方的插件是全部打包的，但通常只使用部分函数，所以通过配置，能够只打包用到的模块。

- babel文件体积：对于一些辅助函数，可能在多个文件都用到，但bable在打包时每用到的地方都会插入该函数，导致重复内容过多，通过配置生成一个类似缓存的地方，需要用辅助函数时就去取。

- 图片压缩：通过插件压缩本地静态图片

- code split：默认情况下，webpack是将所有js文件都打包到一个文件中——即便只加载首页，所有的js文件都会被引入。不能按需加载。

  * 使用多入口，几个入口就会有几个输出。同时多入口方式可以通过配置分割的chunk，将公共的代码额外打包出来。
 
  * 按需加载，对于点击事件这种需要触发的事件函数，可以在触发的时候再加载。这是需要使用`import()`动态导入的方式进行处理，不能将函数文件通过普通方法进行导入。
 
  * 单入口情况时，直接配置chunk。使用react脚手架的项目，配置懒加载路由的情况下，都会有chunk。

- preload/prefetch：前者让浏览器立即加载，后者让浏览器空闲加载，但只加载不执行。但兼容性较差。前者优先级高。

- Network Cache：将文件以来的哈希值存入另一个专门存哈希的文件，减少引用文件变化次数。

- Core-js：处理JS兼容性问题，一般的bable.js没法处理所有的高级语法情况。专门处理ES6以上的语法。

- PWA：渐进式网络应用程序，能够提供类似原生应用体验的Web App技术。提供在**离线**状态下也能够继续运行功能。兼容性较差。
