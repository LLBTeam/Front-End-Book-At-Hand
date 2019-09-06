# Webpack4初体验

## 一、回顾

### 为什么需要打包工具？
[第一个web网页](https://www.w3.org/History/19921103-hypertext/hypertext/WWW/TheProject.html)
* Ajax、HTML5、CSS3 => 业务场景复杂 => 全局变量冲突风险、人工维护依赖关系 => 前端模块化（标准：CommonJS、AMD、CMD）（工具：RequireJS 、SeaJS）
* 开发效率 => html/css/js预编译语言 => 解析插件 => 插件管理 => 前端工程化
* 性能优化（代码拆分、混淆压缩） => 插件管理 => 前端工程化（Grunt、Gulp、Browserify、Webpack）

### 打包工具是什么？
顾名思义，打包工具就是负责把一些分散的小模块，按照一定的规则整合成一个大模块的工具，与此同时，打包工具也会处理好模块之间的依赖关系，最终这个大模块将可以被运行在合适的平台中。
打包工具会从一个入口文件开始，分析它里面的依赖，并且再进一步地分析依赖中的依赖，不断重复这个过程，直到把这些依赖关系理清为止。
从上面的描述可以看到，打包工具最核心的部分，其实就是处理好模块之间的依赖关系。

## 二、Webpack
本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(static module bundler)。在 webpack 处理应用程序时，它会在内部创建一个依赖图(dependency graph)，用于映射到项目需要的每个模块，然后将所有这些依赖生成到一个或多个bundle。

### 优点
* 模块化：在webpack看来一切都是模块！这就是它不可不说的优点，包括你的JavaScript代码，也包括CSS和fonts以及图片等等等，只有通过合适的loaders，它们都可以被当做模块被处理。
* 类似于TypeScript这种在JavaScript基础上拓展的开发语言：使我们能够实现目前版本的JavaScript不能直接使用的特性，并且之后还能能装换为JavaScript文件使浏览器可以识别；
* scss，less等CSS预处理器
* 功能丰富的插件

### 缺点
* 配置太繁琐
* 出了问题无法调试

## 三、Webpack4 的新东西

### 3.1 前提条件
使用最新的 `node.js` 版本，升级 `npm` 到最新（建议node版本安装到8.2以上）

``` bash
npm install -g npm@latest
```

不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。

### 3.2 零配置启动

* 1.创建一个新的文件夹并进入
``` dash
mkdir webpack-4-quickstart && cd $_
```

* 2.初始化 package.json
``` dash
npm init -y
```

* 3.添加 webpack4
``` dash
npm i webpack --save-dev
```

* 4.我们还需要 webpack-cli，它作为一个包独立存在
``` dash
npm i webpack-cli --save-dev
```

* 5.现在打开 package.json 文件，添加 build 脚本
``` dash
"scripts": {"build": "webpack"}
```

* 6.创建 ./src/index.js
``` dash
console.log(`Hello Webpack4`);
```

* 7.运行 build 命令
``` dash
npm run build
```

### 3.3 生产和开发模式

* 打开package.json文件在script部分写入

```
"scripts": {"dev": "webpack --mode development", "build": "webpack --mode production"}
```

```
npm run dev
```

```
npm run build
```

**development** 模式下，将侧重于功能调试和优化开发体验，包含如下内容：
* 浏览器调试工具
* 开发阶段的详细错误日志和提示
* 快速和优化的增量构建机制

**production** 模式下，将侧重于模块体积优化和线上部署，包含如下内容：
* 开启所有的代码优化
* 更小的bundle大小
* 去除掉只在开发阶段运行的代码
* Scope hoisting 和 Tree-shaking
* 自动开启 uglifyjs 对代码进行压缩

### 3.4 移除 loaders，必须使用 rules [webpack4-demo](https://github.com/kongyajie/webpack4-demo)

在webpack3.x中还保留之前版本的loaders，与rules并存都可以使用，在新版中完全移除了loaders，必须使用rules。

### 3.5 支持ES6模块化的方式导入JSON文件，并且可以过滤无用的代码

``` json
// teset.json
{ "a": 1, "b": 2, "data": 3}
```

``` javascript
import { data } from 'test.json'
console.log(data) //3
```

### 3.6 移除CommonsChunkPlugin，用optimization.splitChunks和optimization.runtimeChunk来代替

从webpack4开始官方移除了commonchunk插件，改用了optimization属性进行更加灵活的配置，这也应该是从V3升级到V4的代码修改过程中最为复杂的一部分，下面的代码即是optimize.splitChunks 中的一些配置参考：
研发团队 > Webpack4 初体验 > image2019-1-10 10:30:4.png研发团队 > Webpack4 初体验 > image2019-1-10 9:43:17.png

### 3.7 将CSS提取到文件中：extract-text-webpack-plugin => mini-css-extract-plugin

### 3.8 总体上看

* 集成了常用配置
* 规范了一些配置的写法
* 精简了一些插件

## 四、Webpack3.x 迁移到 Webpack4.x

[Webpack4升级指南](https://segmentfault.com/a/1190000014247030?utm_source=tag-newest)

[从webpack2/3迁移到4](https://webpack.js.org/migrate/4/)

[Webpack3.X-4.X升级记录](https://blog.csdn.net/qq_16559905/article/details/79404173)

[【翻译】Webpack4从0配置到生产模式](https://www.cnblogs.com/zheng-chuang/p/9129630.html)


## 扩展阅读（webpack-chain-demo）

[vue-cli3 中修改webpack配置](https://cli.vuejs.org/zh/guide/webpack.html#%E7%AE%80%E5%8D%95%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%B9%E5%BC%8F)
[webpack-chain](https://github.com/neutrinojs/webpack-chain)


## 其他主题

* 前端模块化发展史（标准、工具）
* 前端打包工具对比
* webpack-chain

