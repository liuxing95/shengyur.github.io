title: Webpack(一)——入门实践与优化配置
date: 2018/11/03
categories: 效率工具
toc: true
tags:
  - Webpack
---

Webpack 4 前端工程化常用配置

![](https://raw.githubusercontent.com/shengyur/Images/master/img/webpack/webpack.png)

## 什么是 webpack

webpack 可以看做是模块打包机：他做的事情是，分析你的项目结构，找到 `JavaScript` 模块以及其他的一些浏览器不能直接运行的扩展语言（`Scss`、`TypeScript` 等），将其打包为合适的格式以供浏览器使用

构建就是把源代码转换成发布到线上可执行的 `JavaScript`、CSS、HTML 代码，包括以下内容：

- **代码转换**：`TypeScript` 编译成 `JavaScript`、`SCSS` 编译成 CSS 等等
- **文件优化**：压缩 `JavaScript`、CSS、HTML 代码，压缩合并图片等
- **代码分割**：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载
- **模块合并**：在采用模块化的项目有很多模块和文件，需要构建功能把模块分类合并成一个文件
- **自动刷新**：监听本地源代码的变化，自动构建，刷新浏览器
- **代码校验**：在代码被提交到仓库前需要检测代码是否符合规范，以及单元测试是否通过
- **自动发布**：更新完代码后，自动构建出线上发布代码并传输给发布系统。

构建其实是工程化、自动化思想在前端开发中的体现。把一系列流程用代码去实现，让代码自动化地执行这一系列复杂的流程。

### webpack 的基本概念

- [入口(entry point)](https://www.webpackjs.com/concepts/entry-points/): 指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始，webpack 会找出有哪些模块和 library 是入口起点（直接和间接）依赖的。

  - 默认值是 `./src/index.js`，然而，可以通过在 webpack 配置中配置 entry 属性，来指定一个不同的入口起点（或者也可以指定多个入口起点）。

- [出口 output](https://www.webpackjs.com/concepts/output/): 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，主输出文件默认为 `./dist/main.js`，其他生成文件的默认输出目录是 `./dist`

- [loader](https://www.webpackjs.com/concepts/loaders/): 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

> 注意，loader 能够 import 导入任何类型的模块（例如 .css 文件），这是 webpack 特有的功能，其他打包程序或任务执行器的可能并不支持。我们认为这种语言扩展是有很必要的，因为这可以使开发人员创建出更准确的依赖关系图。

- [插件 plugins](https://www.webpackjs.com/concepts/plugins/): loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

- [模式 mode](https://www.webpackjs.com/concepts/mode/): 通过选择 `development` 或 `production` 之中的一个，来设置 mode 参数，你可以启用相应模式下的 webpack 内置的优化

### 开发环境和生产环境

我们在日常的前端开发工作中，一般都会有两套构建环境：一套开发时使用，一套供线上使用。

- **development**: 用于开发的配置文件，用于定义 `webpack dev server` 和其他东西
- **production**: 用于生产的配置文件，用于定义 `UglifyJSPlugin`，`sourcemaps` 等

简单来说，开发时可能需要打印 debug 信息，包含 `sourcemap` 文件，而生产环境是用于线上的即代码都是压缩后，运行时不打印 debug 信息等。譬如 axios、antd 等我们的生产环境中需要使用到那么我们应该安装该依赖在生产环境中，而 `webpack-dev-server` 则是需要安装在开发环境中

平时我们 `npm` 中安装的文件中有 -S -D, -D 表示我们的依赖是安装在开发环境的，而-S 的是安装依赖在生产环境中。

本文就来带你搭建基本的前端开发环境，前端开发环境需要什么呢？

- 构建发布需要的 HTML、CSS、JS、图片等资源
- 使用 CSS 预处理器，这里使用 less
- 配置 babel 转码器 => 使用 es6+
- 处理和压缩图片
- 配置热加载，HMR

以上配置就可以满足前端开发中需要的基本配置。


> --`mode` 模式 (必选，不然会有 `WARNING`)，是 `webpack4` 新增的参数选项，默认是 `production`

- `--mode production` 生产环境
  - 提供 `uglifyjs-webpack-plugin` 代码压缩
  - 不需要定义 `new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") })` 默认 `production`
  - 默认开启 `NoEmitOnErrorsPlugin -> optimization.noEmitOnErrors`, 编译出错时跳过输出，以确保输出资源不包含错误
  - 默认开启 `ModuleConcatenationPlugin` -> `optimization.concatenateModules`, `webpack3` 添加的作用域提升(`Scope Hoisting`)
- `--mode development` 开发环境
  - 使用 eval 构建 module, 提升增量构建速度
  - 不需要定义 `new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") })` 默认 `development`
  - 默认开启 `NamedModulesPlugin -> optimization.namedModules` 使用模块热替换(HMR)时会显示模块的相对路径


### 使用：
开发环境：npm run dev
发布编译：npm run build


### 优化：

**Code Splitting**

Code Splitting 是 webpack 一项重要的编译特性，能够帮助我们将代码进行拆包，抽取出公共代码。利用这项特性我们可以做更多的优化工作，减少加载时间，例如可以做按需加载。
此处使用配置为
```
 optimization:{
        splitChunks: {
            cacheGroups: {
              commons: {
                // 抽离自己写的公共代码
                chunks: 'initial',
                name: 'common', // 打包后的文件名，任意命名
                minChunks: 2, //最小引用2次
                minSize: 0 // 只要超出0字节就生成一个新包
              },
              styles: {
                name: 'styles', // 抽离公用样式
                test: /\.css$/,
                chunks: 'all',
                minChunks: 2,
                enforce: true
              },
              vendor: {
                // 抽离第三方插件
                test: /node_modules/, // 指定是node_modules下的第三方包
                chunks: 'initial',
                name: 'vendor', // 打包后的文件名，任意命名
                // 设置优先级，防止和自定义的公共代码提取时被覆盖，不进行打包
                priority: 10
              }
            }
        }
    },
```
详细配置请参考：[SplitChunksPlugin](https://webpack.js.org/plugins/split-chunks-plugin/#splitchunks-chunks)

**Tree Shaking**
如果你对自己编写的代码很了解，你可以通过在 package.json 中添加 sideEffects 来启用 Tree Shaking ，即摇树优化，帮助我们删掉一些不用的代码。这里不再赘述，详情可以点击[Tree Shaking](https://webpack.js.org/guides/lazy-loading/)。

**Dynamic import**
在谈到 Code Spliting 时，我们不得不想到 dynamic import ，在之前版本的 webpack 中，我们想实现动态加载使用的是 require.ensure ，而在新版本中，取而代之的 import() ，这是TC39关于使用 import()的[提案](https://github.com/tc39/proposal-dynamic-import)，而目前 import()兼容性如下：

import() 返回一个 Promise ，如果你想使用它请确保支持 Promise 或者使用 Polyfill ，在想使用 import() 前，我们还得使用预处理器，我们可以使用 [@babel/plugin-syntax-dynamic-import](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import/#installation)  插件来帮助webpack解析。webpack 官方给了我们一个 dynamic import 的[示例](https://webpack.js.org/guides/code-splitting/#dynamic-imports) ，这里我就不做举例。使用 import() 我们可以很方便的实现 preload 预加载、懒加载以及上面谈到的 Code Splitting。

```
<!DOCTYPE html>
<nav>
  <a href="books.html" data-entry-module="books">Books</a>
  <a href="movies.html" data-entry-module="movies">Movies</a>
  <a href="video-games.html" data-entry-module="video-games">Video Games</a>
</nav>

<main>Content will load here!</main>

<script>
  const main = document.querySelector("main");
  for (const link of document.querySelectorAll("nav > a")) {
    link.addEventListener("click", e => {
      e.preventDefault();

      import(`./section-modules/${link.dataset.entryModule}.js`)
        .then(module => {
          module.loadPageInto(main);
        })
        .catch(err => {
          main.textContent = err.message;
        });
    });
  }
</script>
```

**Polyfill**
Polyfill 现在对于大家来说应该并不陌生，他可以帮助我们使用一些浏览器目前并不支持的特性，例如 Promise 。在Babel中，官方建议使用 babel-preset-env 配合 .browserslistrc ，开发人员可以无需关心目标环境，提升开发体验。尤其在 Polyfill 方面，只要我们配置好 [.browserslistrc](https://github.com/browserslist/browserslist) ，Babel 就可以智能的根据我们配置的浏览器列表来帮助我们自注入 Polyfill ，比如：
.babelrc
这里使用的是
```
{
    "presets": [
        [
            "@babel/preset-env",
            {
                "modules":false,
                "targets":{
                    "browsers":["> 1%", "last 2 versions", "not ie <= 8"]
                },
                "useBuiltIns":"usage"
            }
        ]
    ]
}
```
[useBuiltIns](https://babeljs.io/docs/en/babel-preset-env#usebuiltins) 告诉 babel-preset-env 如何配置 Polyfill 。
每个浏览器之间的特性具有很大的差异，为了尽可能的减小包的大小，我们可以为每个主流浏览器单独生成 Polyfill ，不同的浏览器加载不同的 Polyfill 。
这里我配置为：usage，在每个文件中使用polyfill时，为polyfill添加特定导入（根据浏览器环境来自动判断是否import polify）这样就完成了自动根据 .browserslistrc注入 Polyfill。

**首屏文件**
SPA 程序打包出来的html文件一般都是很小的，也就2kb左右，似乎我们还可以利用下这个大小做个优化，有了解[初始拥塞窗口](https://tylercipriani.com/blog/2016/09/25/the-14kb-in-the-tcp-initial-window/) 的同学应该知道，通常是14.6KB，也就意味着这我们还能利用剩下的12KB左右的大小去干点什么，这了我建议内联一些首屏关键的css文件（可以使用 criticalCSS ），或者将css初始化文件内联进去，当然你也可以放其他东西，这里只是充分利用下初始拥塞窗口 特性。
这里顺便讲下css初始化，css初始化有很多种选择，其中有三种比较出名的，分别是：[normalize.css](https://github.com/csstools/normalize.css) 、[sanitize.css](https://github.com/csstools/sanitize.css) 和 [reset.css](https://meyerweb.com/eric/tools/css/reset/) 。关于这三种的区别我就直接引用了。
```
normalize.css and sanitize.css correct browser bugs while carefully testing and documenting changes. normalize.css styles adhere to css specifications. sanitize.css styles adhere to common developer expectations and preferences. reset.css unstyles all elements. Both sanitize.css and normalize.css are maintained in sync.
```

**缓存**
使用chunkhash与contenthash





参考：
https://juejin.im/post/5bbc1b0c6fb9a05cf230140c