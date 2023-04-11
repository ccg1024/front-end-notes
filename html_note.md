**self closed tag**

- img
- input
- meta
- link

```js
// window > document
/*
* window object represent the browser window which opened.
* the document object represent the doc which writed.
* so, window.document will show the doc content in current window tab.
* so, if there is only one tab and one web page,
* the document should equal to window.document
* document object could be see as a ancestry tree.
*/
```

节点中的文字部分，如下所示，视为`textNode`。

```html
<p>This text is a textNode</p>
```

html5中添加了一种规范，以`data-`开头的属性都是自定义的属性。

### HTML5新特性

html5总体上增加了10个特性，不支持ie8及以下。

1. 语义标签：方便开发者清晰构建页面。如同`main`标签代替主区域的`div`标签

2. 增强型表单：修改一些`input`的输入特性，更好的输入控制与验证——通过`type`属性来判断，新增了表单标签元素与表单属性。

3. 视频和音频：提供了音频与视频文件的标准——`audio`与`video`

4. `canvas`绘图：可通过JS在其中绘制图像

5. `svg`绘图：可伸缩矢量图形。

   > 与canvas差别，svg适用于xml中的2D图形，canvas可随时随地绘制2D图形，svg基于XML，是DOM操作，svg每个图像是一个对象，发生改变，页面重绘。canvas是一个像素一个像素的改变，改变一个位置，整个画布重绘。

7. 地理定位：通过`getCurrentPosition()`可以获取用户地理位置

8. 拖放API：这是HTML5中的标准，任何元素都能够被拖放，`draggable`属性控制

9. `WebWorker`：通过加载脚本，创建一个独立工作的线程，于主线程之外运行。拥有独立的事件循环。

10. `WebStorage`：就是localStorage与sessionStorage。目的是用于解决本不应该用cookie处理的本地存储，但却使用cookie存本地信息的场景。

11. `WebSocket`：为web客户端与服务端之间提供的一种全双工通信机制。特点

    - 握手阶段采用HTTP协议，默认端口为80和443
    - 建立在TCP协议基础之上，为应用层协议
    - 可以发送文本与二进制数据
    - 没有同源限制
    - 协议标识符为`ws`，加密协议为`wss`。如：`ws://localhost:8080`

为什么会有`WebSocket`，首先socket编程就是为了使用TCP/IP协议，本身socket不是协议，而是对下面协议的封装，通过socket编程，构建目标IP与端口套接字，可以实现应用TCP的网络连接。

虽然HTTP是基于TCP的应用层协议，但是，是出于半双工状态——工作方式为请求-响应，无法随时的双向通信。而`WebSocket`则是基于TCP建立的全双工协议，解决HTTP无法实时通信的问题。一旦建立连接，双发可以随时发送数据。
