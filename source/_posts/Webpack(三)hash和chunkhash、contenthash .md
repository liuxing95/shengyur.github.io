title: Webpack(三)—hash和chunkhash、chunkhash的使用场景与区别
date: 2018/12/01
categories: 效率工具
toc: true
tags:
  - Webpack
---

## webpack中hash和chunkhash有何不同？(contenthash)

### 两者的使用
```
//使用hash的情况
output: {
  path: path.resolve(__dirname, '../dist/static'),
  publicPath: '/',
  filename: 'static/js/[name].[hash].js',
  chunkFilename: 'static/js/[id].[hash].js'
}
//使用chunkhash的情况
output: {
  path: config.prod.assetsRoot,
  filename: 'static/js/[name]-[chunkhash:16].js',
  chunkFilename: 'static/js/[id]-[chunkhash:16].js',
  publicPath: '/'
},
```
### hash和chunkhash是什么
大家都知道，用于优化页面性能的常用方法就是利用浏览器的缓存，文件的hash值，就相当于一个文件的身份证一样，很适用于前端静态资源的版本管理，前端实现增量更新的方案之一，那为什么会有两个东西呢，先看两者的定义

- hash

[hash] is replaced by the hash of the compilation. 
compilation的hash值

- chunkhash
[chunkhash] is replaced by the hash of the chunk.
chunk的hash值
后者很容易理解，因为chunk在webpack中的含义就是模块，那么chunkhash根据定义来就是**根据模块内容计算出来的hash值**。
在理解前者之前我们先来看一下compilation有什么作用

**compilation的浅析**

webpack的官网文档中HOW TO WRITE A PLUGIN中对这个有一段文字的解析

> A compilation object represents a single build of versioned assets. While running Webpack development middleware, a new compilation will be created each time a file change is detected, thus generating a new set of compiled assets. A compilation surfaces information about the present state of module resources, compiled assets, changed files, and watched dependencies. The compilation also provides many callback points at which a plugin may choose to perform custom actions.

翻译：
> compilation对象代表某个版本的资源对应的编译进程，当你跑webpack的development中间件，每当检测到一个文件被更新之后，一个新的comilation对象会被创建，从而引起新的一系列的资源编译。一个compilation含有关于模块资源的当前状态、被编译的资源，改变的文件和监听依赖的表面信息。compilation也提供很多回调方法，在一个插件可能选择执行制定操作的节点

而与compilation对应的还有一个compiler对象，我们也来介绍一下，这样能够更方便理解compilation

> The compiler object represents the fully configured Webpack environment. This object is built once upon starting Webpack, and is configured with all operational settings including options, loaders, and plugins. When applying a plugin to the Webpack environment, the plugin will receive a reference to this compiler. Use the compiler to access the main Webpack environment.

翻译：
> compiler对象代表的是整个webpack的配置环境，这个对象只在webpack开始的时候构建一次，且所有的操作设置包括options，loaders，plugin都会被配置，当在webpack中应用插件时，这个插件会接受这个compiler对象的引用。通过webpack的主环境去使用这个compiler。

简单的说，compiler是针对webpack的，是不变的webpack环境，而compilation这个就是每次有一个文件更新，然后会重新生成一个，那么当你一个文件的更新，所有的hash字段都会发生变化，这就很坑了，本来我们做增量更新就是想改的那个文件发生变化，但是如果全部都发生变化就没有意义了，我们来看一下实际操作中的例子：
![hashoutput1](https://raw.githubusercontent.com/shengyur/Images/master/img/webpack/hashOutput.png)
![hashoutput2](https://raw.githubusercontent.com/shengyur/Images/master/img/webpack/hashoutput2.png)

而且从上图中可以看出，每次有文件更新，会产生一个新的compilation，从而会用新的compilation来计算得出新的hash，而且每个文件带有的hash值还是一样的，这样的肯定达不到我们的要求，那么如何避免这个问题呢？——chunkhash

### chunkhash是如何生成的
chunkhash是由chunk计算的得出的hash值，chunk指的是模块，这个hash值就是模块内容计算出来的hash值
修改单个文件前的chunkhash:
![](https://raw.githubusercontent.com/shengyur/Images/master/img/webpack/chunkhash2.png)

修改后的文件的chunkhash
![](https://raw.githubusercontent.com/shengyur/Images/master/img/webpack/chunkhash22.png)


这里我们还得提一个问题，比如像vue这些框架，把js和css共同放在一个里面会时，我们一般会用一个插件叫extract-text-webpack-plugin/mini-css-extract-plugin，这样我们就能把css单独打成一个包，但是这样就会产生一个问题，这样打包出来的css的chunkhash和js的chunkhash会不会是一样的呢，其实我这么问了，当然是会的啦。

其实也很简单，webpack的理念就是为了js的打包，style标签也会视为js的一部分，那么这我们会发现，还是有坑，当我们只改css的时候，js也会同时发生改变，那么我们还是没有做到严格意义上的增量更新，那么我们又该怎么解决呢？

### contenthash
使用方式如下：
```
new MiniCssExtractPlugin({
            filename: '[name].[chunkhash:8].css'
          })
```

这样就可以实现资源的合理跟新了

参考：
https://blog.csdn.net/bubbling_coding/article/details/81561362
https://github.com/laihuamin/JS-total/issues/19

