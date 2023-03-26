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
