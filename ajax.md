# Ajax

> Ajax——Asynchronous Javascript And XML

前端常用的异步请求库，在React应用中使用进一步封装后的Axios。

通常，在网络上向服务请通过Web协议进行数据请求时都会用到`XMLHttpRequest`对象。

```js
// 原生的方法
let xhr = new XMLHttpRequest()
```
* * * *

JQuery 中常用的Ajax方法。

- `$.get(url, [data], [callback])`：data为携带的参数，callback是请求成功的回调函数。

- `$.pose(url, [data], [callback])`：参数内容同`get`方法。

- `$.ajax(obj)`：既可以`get`也可以`post`。obj对象内容如下

  - `type: ''`：请求的方式
  - `url: ''`：请求的地址
  - `data:{}`：请求携带的参数
  - `success: function(res){}`：请求成功后的回调函数

* * * *

### XMLHttpRequest

**发送GET请求的步骤**

1. 创建XMR对象
2. 调用`open`函数
3. 调用`send`函数
4. 监听`onreadystatechange`事件

```js
let xhr = new XMLHttpRequest()
xhr.open('GET', 'https://www.baidu.com?q=1')
xhr.send()

xhr.onreadysatechange = function() {
  if (xhr.readyState === 4 && xhr.status === 200) {
    // 服务器数据
    console.log(xhr.responseTest)
  }
}
```

`readyState`属性是XHR对象的请求状态，值是`0~4`,4表示请求完成——不论成功与否。

**发送POST请求步骤**

1. 创建对象
2. 调用`open`函数
3. *设置`Content-Type`属性*
4. 调用`send`函数，*同时指定发送的数据*
5. 监听`onreadystatechange`事件

```js
let xhr = new XMLHttpRequest()
xhr.open('POST', 'https://www.baidu.com')
// 固定写法
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded')

xhr.send('id=1&name=tom')

xhr.onreadystatechange = function() {
  if(xhr.readyState===4&&xhr.status===200) {
    console.log(xhr.responseText)
  }
}
```

### 状态码

- 201：已创建，成功请求并创建资源，常用与PUT,POST请求的返回。

  > 3**开头的为重定向

- 301：永久移动，请求资源被永久移动到新的URL，返回新的RUL，浏览器自动定向

- 302：临时移动，与301类似，但资源只是临时移动

- 304：未修改，同一个URL的get请求，若服务器已经响应。后续收到对该文档的请求时，若文档相对于上一次请求没变，则服务器返回304，表示服务器已执行了get，但文档未变化。

  > 4**开头的客户端错误相关

- 400：请求语义有误或请求参数有误

- 401：当前请求需要用户验证

- 403：服务器已理解请求，但拒绝执行

- 404：服务器无法根据客户端请求找到资源

- 408：请求超时。服务器等待客户端发送的请求时间过长

  > 5**开头的服务端错误

- 500：服务器内部错误，无法完成请求

- 501：服务器不支持该请求方式，无法完成请求（只有GET与HEAD是每个服务器必有的）

- 503：由于超载或系统维护，服务器暂时无法处理请求


### URL编码

非英文字符会被转化——中文字符无法出现在URL中，JS中两个原生的方法可以实现浏览器的转化过程。

`encodeURL('中文')`, `decodeURL('%E9%BB%91')`分别进行编码与解码。

### XMR的新特性

H5中新增了`FormData`对象，能够直接从表单元素中获取所有的输入信息。`let p = new FormData(form)`。同时，XML能够上传文件——通过POST方法。

### 同源策略优化与跨域

同源：两个页面的协议，域名，端口都相同。**同源策略**是浏览器提供的一个安全功能。规定不同源之间的脚本资源不能进行资源交互（**Ajax请求也是不允许的**）。

跨域就是违反同源策略的行为。浏览器默认会对跨域请求进行拦截，**但不是不能够发送请求**。跨域的Ajax请求是能够正常发出的，目标接口返回数据后，**浏览器同源策略会对数据进行拦截**。

以前实现跨域请求的方式**JSONP**, **CORS**，现在配置两WebPack的项目可以通过**代理**实现跨域请求。

**JSONP**年代早，能够兼容低版本IE，临时解决方案。只能够支持GET请求。

> 利用`script`标签中src属性不受同源策略影响的特性实现，就是将请求写道src上。

**CORS**出现较晚，是W3C标准，属于跨域Ajax的根本解决方案，支持GET与POST请求。不兼容低版本IE。

> 如今CORS配置实际上也会有些小问题。
