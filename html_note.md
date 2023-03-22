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