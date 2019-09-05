# 1.4：HTML标签语义化

## 语义化是什么、为什么、怎么做

简单来说，HTML 语义化就是：
> 根据结构化的内容，选择合适的标签。

那么为什么要做到语义化呢？

直观上很好理解，“合适的标签”是内容表达的高度概括，这样浏览器爬虫或者任何机器在读取 HTML 时，都能更好地理解，进而解析效率更高。这样带来的收益如下：

* 有利于 SEO
* 开发维护体验更好
* 用户体验更好（如使用 alt 标签用于解释图片信息）
* 更好的 accessibility，方便任何设备解析（如盲人阅读器）

那么如何做到语义化呢？

其实很简单，这就要求我们实时跟进、学习并使用语义化标签。这里我帮大家总结了一些典型的 HTML 标签，并进行分类。

![enter image description here](https://images.gitbook.cn/2aa65430-88e6-11e9-8115-8f03dd50b734)

我将 HTML 标签分为 9 大类别，每一种类别都包含有语义化的标签内容，小图如下：

![enter image description here](https://images.gitbook.cn/50392a10-88e6-11e9-b157-776f1929e232)

![enter image description here](https://images.gitbook.cn/78d71d10-88e6-11e9-88f0-f1d5a1392b19)

![img](https://images.gitbook.cn/ef3dc850-88e6-11e9-8115-8f03dd50b734)

了解了这些语义化的标签，我们就可以按照“适合内容与否”，进行使用。关于选取标准，我也简单总结了一下，抽象成代码表达为：

```javascript
if (导航) {
  return <nav />
}
else if (文稿内容、博客内容、评论内容...包含标题元素的内容) {
  return <article />
}
else if (目录抽象、边栏、广告、批注) {
  return <aside />
}
else if (含有附录、图片、代码、图形) {
  return <figure />
}
else if (含有多个标题或内容的区块) {
 return <section />
}
else if (含有段落、语法意义) {
  return <p /> || <address /> || <blockquote /> || <pre /> || ...
}
else {
  return <div />
}

```



## 语义化的发展和高级玩法

说到语义化的发展，我这里指向重点提一个概念：Microformats，如果面试官问的语义化时，你能把这个概念搬出来，效果是非常好的。那什么是 Microformats 呢？

> Microformats，翻译为微格式，是 HTML 标记某些实体的小模式，这些实体包括人、组织、事件、地点、博客、产品、评论、简历、食谱等。它们是在 HTML 中嵌套语义的简单协议，且能迅速地提供一套可被搜索引擎、聚合器等其他工具使用的 API。

除了 hCard 和 hCalendar，有好几个库特别开发了微格式。

是不是看的一脸懵逼？其实很简单，Microformats 的原理就是扩展 HTML 元素或者属性，来增强 HTML 的语义表达能力。

我们来看一个案例：

![enter image description here](https://images.gitbook.cn/4bd676c0-88e7-11e9-8115-8f03dd50b734)

Wikipedia 的页面中，给某一部分加上了 vCard 的 class，这是用来做什么的呢？

![enter image description here](https://images.gitbook.cn/d8469cc0-88e7-11e9-b068-5302ecb579f2)

Google 搜索引擎可以通过 Wikipedia 页面 vCard 这个 class，读取相关内容，在呈现搜索结果时，匹配展现出人物信息。从而语义化的 class，帮助了机器（搜索爬出）学习到更多信息，展现出了更好的结果页面。

Microdata 属于 WHATWG（网页超文本应用技术工作小组：Web Hypertext Application Technology Working） HTML 规范，它并不是标准，但这是一个很典型的语义化发展和应用尝试。