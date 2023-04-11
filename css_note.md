### 伪类与伪元素

是附加在选择器末的关键词，运行对被选则元素的特定部分进行修改。一个选择器只能跟一个伪元素，通常双冒号表示伪元素，单冒号表示伪类。

```css
/*pseudo class*/
p:hover {
  
}

/*pseduo element*/
p::first-line {
  
}
```

CSS优先级的原则(对于相同元素的不同规则)

- css文件中的位置，通常生效最后一个。
- 选择器越细节的优先级越高。通常 tag < class < id
  常用的计算方式
  - 有几个id
  - 有几个class
  - 有几个tag
  
    ```css
    // 优先级 001
    p {}
    // 优先级 011
    p .class1 {}
    // 优先级 111
    p .class1 #id1 {}
    // 优先级 200
    #id1 #id2 {}
    ```
> 直接写在html中的内联样式优先级是最高的；
> \<p style="color: blue;">fisrt of all</p>

盒模型：就是一个元素分为: content, padding, border, margin.

**display元素可取值**

- block（独占一行）(take all space of its container)
- inline (无法手动设置高宽)
- inline-block
- flex
- grid
- none（不会占用空间）

在盒模式中，有两种大小计算方式，box-sizing: content-box/border-box，默认为content-box。文档解释参考[box-sizing](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing)

**position**

- static (default)
- absolute (remove out normal dom flow)
- relative (in normal dom flow, and will remain space for it)
- fixed (remove out normal dom flow)
- sticky (newer) (like fixed, but in normal dom flow)

> width: 100% equal to width: 100vw, (means 100% viewpoint width)

在默认情况下：top，bottom，left，right，z-index是无效的。元素定义relative后，原来位置的空间依旧会预留，此时，与默认情况无差别，只是多了z-index属性。若定义top等数值，则会产生对应的偏移。定义absolute时，则不会预留空间，会从文档流中移除，从最近的非static祖先处定义位置。若定义fixed，同样移除文档流，不会预留空间，根据viewpoint定义位置。

**CSS measurement Units**

1. pixels
2. em
3. rem
4. percentages

在定义字体大小时，为了方便管理，通常在全文上定义一个正常大小的字体。例如

```css
html {
  font-size: 16px;
}
```

依据上面的定义，若设置字体为`1rem`表示字体大小为100%的根节点字体大小，即16px。而`em`则是相对于父节点的字体大小，表示内容同`rem`。只不过参考的对象不同。

> margin: top right bottom left

> 使用inline-block，让两个宽度为50%的元素放在同一行，但实际效果却没有。是因为在源码中，两个元素之间存在回车或空格。当源码是贴着写的时候，代码表现就如预期了。（应该算是bug）

### 媒体查询

1. min-width: 当宽度大于等于该数值时，即生效的最小宽度
2. max-width: 当宽度小于等于该数值时，即生效的最大宽度

mobile first vs desktop first，只是在写css的阶段。

### FlexBox

新型设计模式，让元素在一行或一列上展示。通常，将父节点的display设置成flex时，子节点将自动在一行上显示。(flex-container, flex-item)

**Flex Container Properties**

1. display
2. flex-direction：(默认为row)
3. justify-content：(main axis水平方向对齐规则)
4. align-items：(flex-item的对齐方式)
5. flex-wrap
6. align-content：(cross axis垂直方向对齐规则)

**Flex Item Properties**

1. align-self
2. order：子元素的显示顺序
3. flex-grow: 对于空白区域，子元素占用的份额，默认为0，（当子元素没有填满父元素时）
   > 若有4个子元素，都设置为1。则父元素的空白空间被分成4份，每个子元素占一份。(默认为0)
4. flex-shrink：当子元素已经填满，但还有多于的子元素时，每个子元素缩小的比例。
   > 例如3个子元素宽度为50，已经填满，那么第4个子元素就没位置。父元素缺少50宽度。此时，缺少的50会被分成4份，每个子元素缩小一份的宽度。（默认值为1）。但不会缩小到无法容纳子元素自身的内容大小。有下限。
5. flex-basis：对应子元素在主轴上的大小。(默认值为auto)

> flex是一维的，grid是二维的。 flex是flex-grow flex-shrink flex-basis缩写

### CSS3新特性

css（cascading style sheets）层叠样式表，一种标记语言。css3是向后兼容的，即以前的语法也可以使用。

新增了一些**选择器**，例如属性选择器中使用部分的正则——以那些字母开头。新增了一些样式。例如圆角边框，阴影，背景属性（背景大小），颜色（rgba，hala）。

**transition过渡**

```css
transition： CSS属性，花费时间，效果曲线(默认ease)，延迟时间(默认0)
/* or */
transition-property: width; 
transition-duration: 1s;
transition-timing-function: linear;
transition-delay: 2s;
```

**transform转换**

- transform: translate(12px, 12px) 位移

- transform：scale(2, 0.5) 缩放

- transform：rotate(0.5turn) 旋转

- transform：skew(30deg, 20deg) 倾斜

**animation动画**

结果可能跟transition差不多，但animation好像有GPU加速。

- animation-name：动画名称
- animation-duration：动画持续时间
- animation-timing-function：动画时间函数
- animation-delay：动画延迟时间
- animation-iteration-count：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环
- animation-direction：动画执行方向
- animation-paly-state：动画播放状态
- animation-fill-mode：动画填充模式

**颜色渐变**，**flex**等。