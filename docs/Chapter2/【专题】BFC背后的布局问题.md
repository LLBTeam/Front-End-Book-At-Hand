# 【专题】BFC背后的布局问题

## 谈论BFC之前你需要知道的事情

### 浏览器渲染/布局机制

我们之前提到过Web的历史，最初它的出现是为了解决信息同步和信息共享的问题。不过随着web的发展，大家对它有了更大的期待。

试想一下，如果你是初代浏览器的设计者，现在你想要把我们在报刊、杂志上看到内容搬到网页上，你会怎么做？

![image-20190805105913547](/../images/docs/G20中美贸易谈判.png)

首先，你就会遇到一个问题：

* 如何渲染文本，比如我们在报刊上看到标题：“中美贸易谈判重启”的大号标题，以及下面的标题
* 如何渲染图片，比如头版头条上G20上特朗普和习近平的握手照片

所以，你需要建立一种渲染机制来绘制文本，支持不同的字号大小、字体类型、字号的粗细等，于是就有了我们现在的`<h1> `和css文本相关的属性；同样的，`<img>`是为了绘制图片而生。



然后问题来了，绘制文本、图片，不是随便乱绘对不对。怎么绘制呢？

option1：从上往下是一个选择！

option2：从下往上也是一个选择！

不过option1更符合我们的浏览习惯，那就选它吧，另外送它一个好听的名字：

**文档流**~



有了文档流，我们可以把文本和图片按照我们想要的顺利依次从上往下摆放，那么问题来了：如果图片和文本想要排在一行，或者希望像有些报刊文章一样文字环绕着图片呢？

**浮动**~



到这里，基本报刊的文章内容已经可以移植到web上了！！

不过，移植报刊内容仅仅只是web的一个功能，它有比报刊更多的功能需求。比如广告，为了让广告可以被放置在web的任意位置，于是有了**绝对定位**；而有些广告或者非常重要的信息要求固定在web的底部或头部，于是还需要**固定定位**；还有**相对定位**，也有它相应的运用场景。



有了文档流、浮动、定位这三大杀器，基本的网页布局也就都能实现了！下面说说渲染机制：

### 机制1：文档流

* 内联元素：又叫行内元素，多个内联元素可以在一行显示，如`<span>` `<img>`
* 块级元素：四四方方的块，在文档中自己占一行，如`<div>`

下面这张图说明了一切：

![img](https://pic3.zhimg.com/80/v2-bcc2930dd73184475467637b4b4dcc66_hd.jpg)

### 机制2：脱离文档流

回到文章开头的例子上面，将文档流比作是河流的话，水就相当于文档流里面的元素。而脱离文档流就相当于**脱离水跑到水的上面飘着，**就像河流上的小船。

![img](https://pic1.zhimg.com/80/v2-26399045d2ee44d3dd28dd9d37fe4848_hd.jpg)

* position: absolute
* position: fixed
* position: relative
* float: left/right

## BFC

### BFC是什么

简单来说，BFC 就是：

BFC 是 Block Formatting Context 的简写，我们可以直接翻译成“块级格式化上下文”。它会创建一个特殊的区域，在这个区域中，只有 block box 参与布局。而 BFC 的一套特点和规则就规定了在这个特殊的区域中如何进行布局，如何进行定位，区域内元素的相互关系和相互作用。这个特殊的区域不受外界影响。

上面提到了 block box 的概念，block box 是指 display 属性为 block、list-item、table 的元素。

顺便插一个问题：那你还知道其他哪些 box 类型呢？

相应地，我们有 inline box，它是指 display 属性为 inline、inline-block、inline-table 的元素。CSS3 规范中又加入了 run in box，这里我们不再展开。

### BFC有什么特性

我们上面谈到了 BFC 的一套规则，那么这些规则都有哪些呢？

- 内部的 box 将会独占宽度，且在垂直方向，一个接一个排列
- box 垂直方向的间距由 margin 属性决定，但是同一个 BFC 的两个相邻 box 的 margin 会出现边距折叠现象
- 每个 box 水平方向上左边缘，与 BFC 左边缘相对齐，即使存在浮动也是如此
- BFC 区域不会与浮动元素重叠，而是会依次排列
- BFC 区域内是一个独立的渲染容器，容器内元素和 BFC 区域外元素不会形成任何干扰
- 浮动元素的高度也参与到 BFC 高度的计算当中

从这些规则中，我们至少能总结出一些关键要点，比如：

- 边距折叠
- 清除浮动
- 自适应多栏布局

这也是我选取 BFC 这个概念来剖析的原因，理解了 BFC，这些常见、常考知识点我们都可以融会贯通，具体来看下下面的场景。

### 如何构建BFC

那么什么样的情况会创建一个 BFC 呢？MDN 总结如下：

- 根元素或其他包含它的元素
- 浮动元素 (元素的 float 不是 none)
- 绝对定位元素 (元素具有 position 为 absolute 或 fixed)
- 内联块 (元素具有 display: inline-block)
- 表格单元格 (元素具有 display: table-cell，HTML 表格单元格默认属性)
- 表格标题 (元素具有 display: table-caption, HTML 表格标题默认属性)
- 具有 overflow 且值不是 visible 的块元素
- display: flow-root 的元素
- column-span: all 的元素

### BFC实战应用

- 例题1

给出如下代码：

```
<style>
    body {
        width: 600px;
        position: relative;
    }

    .left {
        width: 80px;
        height: 150px;
        float: left;
        background: blue;
    }

    .right {
        height: 200px;
        background: red;
    }
</style>

<body>
    <div class="left"></div>
    <div class="right"></div>
</body>
```

我们得到布局如图：

![enter image description here](https://images.gitbook.cn/04b9dab0-88e8-11e9-88f0-f1d5a1392b19)

请在不修改已有内容情况下，加入样式，实现自适应（.left 宽度固定，.right 占满剩下宽度）两栏布局。

我们来思考：根据 BFC 布局规则：“每个 box 水平方向上左边缘，与 BFC 左边缘相对齐。即使存在浮动也是如此”，因此 .left 和 .right 的左边相接触。出现如此布局结果并不意外。

同时，再想想 BFC 布局规则：“BFC 区域不会与浮动元素重叠，而是会依次排列”，因此我们可以使 .right 形成 BFC，来实现自适应两栏布局。如何形成 BFC 前面已经做过介绍了，于是添加：

```
.right {
    overflow: hidden;
}
```

就可以得到：

![enter image description here](https://images.gitbook.cn/a9bd6130-8915-11e9-b157-776f1929e232)

当然，这种布局可以用更先进的 flex 或者 grid 手段解决，但是对于 BFC 这些 CSS 基础知识，同样要做到了然于胸。

- 例题 2

看代码：

```
<style>
    .root {
        border: 5px solid blue;
        width: 300px;
    }

    .child {
        border: 5px solid red;
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="root">
        <div class="child child1"></div>
        <div class="child child2"></div>
    </div>
</body>
```

首先来回答第一个问：**.root 的高度是多少？**

事实上，因为 .child 为浮动元素，因此造成了“高度塌陷”现象，.root 的高度为 0。

![img](https://images.gitbook.cn/ca790c80-8915-11e9-8115-8f03dd50b734)

那么如何解决“高度塌陷”问题呢？

想想 BFC 规则：“浮动元素的高度也参与到 BFC 高度的计算当中”，因此使 .root 形成 BFC，就能解决问题：

```
.root {
    overflow: hidden;
}
```

![img](https://images.gitbook.cn/06df63e0-8916-11e9-8115-8f03dd50b734)

我们看此时高度已经被你撑开了。

- 例题 3

代码：

```
<style>
    p {
        color: blue;
        background: red;
        width: 400px;
        line-height: 100px;
        text-align:center;
        margin: 40px;
    }
</style>
<body>
    <p>paragraph 1</p>
    <p>paragraph 2</p>
</body>
```

首先回答问题：**两段之间的垂直距离为多少？** 想想 BFC 规则：“box 垂直方向的间距由 margin 属性决定，但是**同一个** BFC 的两个相邻 box 的 margin 会出现边距折叠现象”。事实上，因为边距折叠现象，答案为 40px。

那么如何解决这个问题呢？

最简单地，我们可以在 p 标签再包裹一个元素，并触发该元素形成一个BFC。那么这两个 p 标签，不再属于同一个 BFC，从而解决问题。

```
<style>
    p {
        color: blue;
        background: red;
        width: 400px;
        line-height: 100px;
        text-align:center;
        margin: 40px;
    }
    .wrapper {
        overflow: hidden
    }
</style>
<body>
    <p>paragraph 1</p>
    <div class="wrapper">
        <p>paragraph 2</p>
    </div>
</body>
```

![img](https://images.gitbook.cn/2f329240-8916-11e9-b157-776f1929e232)

**总结**：我们通过分析 BFC 是什么、如何形成、布局规则，融会贯通了 CSS 当中很多关键问题。也许不少读者能够解决“边距折叠”、“多栏自适应”、“高度塌陷”等问题，但是并不能说出解决问题的原理。通过这一环节的学习，我们对 CSS 加深了理解，我更希望地是能够启发大家思考：我们到底应该如何对待 CSS、如何学习 CSS。





[一篇文章了解HTML文档流(normal flow)](https://zhuanlan.zhihu.com/p/31261020)