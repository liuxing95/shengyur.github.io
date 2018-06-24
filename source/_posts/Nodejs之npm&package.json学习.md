title: Nodejs之npm&package.json学习
date: 2018/05/18
categories: Nodejs
tags:
  - Nodejs
---

作为前端，因为经常用到gulp，webpack等工具，所以我们最常见到的是npm和package.json，所以先总结一下它们俩的常用命令。

## npm
### 初始化
```
 npm init //会询问package.json的各种信息，从而确认

 npm init --y //全部使用默认值,快速生成package.json
```
### 安装依赖包
```
 npm install <package name> <package name> ...

 npm install <package name> -g

 npm install <package name> --save

 npm install <package name> --save-dev

 npm install <pacakage name>  --O //--save-optional  -B: --save-bundle  -E: --save-exact
```
npm install <package name> -g :全局安装，需要注意的是全局模式并不是将一个模块安装包安装为一个全局包的意思，它并不意味着可以从任何地方通过require()来引用，-g的含义是将一个包安装为全局可用的可执行命令,例如gulp，webpack等。

--save与--save-dev的区别 :
--save:是生产环境中项目运行需要的依赖,比如代码所依赖的库或框架(Jquery,React等)，安装后被记录在package.json中的dependencies关键字下；
--save-dev:开发时候需要的依赖，安装后被记录在devDependencies关键字下。如一些开发时候需要用的gulp，webpack工具。

同样--O/B/E分别会被记录到对应的关键字下。

### 更新依赖包
```
 npm update

 npm update  -g

 npm outdated

 npm outdated -g
```

在项目目录下运行npm update可以升级项目中所用依赖到最新版本，而npm update -g则可以升级全局安装的依赖包到最新版。

npm outdated用于检查模块是否过时并列出。

### 卸载依赖
```
 npm uninstall <package name> <package name> ...

 npm uninstall <package name> -g

 npm uninstall <package name> --save

 npm uninstall <package name> --save-dev
```
使用npm uninstall可以卸载依赖，但是卸载后，在package.json中的纪录并不会被删除，要想在卸载依赖的同时删除在package.json中的纪录，需要在卸载的时候使用安装时的所有的选项，例如，如果安装时候使用了npm install <package name> --save则卸载的时候，同样使用npm uninstall <pacakage name> --save，而如果使用了--save-dev，卸载时候也需要加相同的选项。

### 使用自定义npm命令
在package.json中，有一个scripts关键字，只需要在该关键字内写入自定义命令以及对应执行的实际命令即可。
```
"scripts":{
    "test": "nonde ./test.js",
    "dev": "gulp --gulpfile gulpfile-dev.js",
    "build": "gulp --gulpfile gulpfile-build.js"
}
```
上面的配置中，只要我们在终端运行npm dev就是运行了gulp --gulpfile gulpfile-dev.js，这样就省去了我们在终端输入很长的一段命令，非常方便。

### 其他
npm view <pacakage name>可以查看包的package.json文件，如果只是看包的某个特性，在后面加上相应的key即可，例如npm v zepto version就是查看当前安装的zepto的版本，v是view的简写。

npm ls可以分析出当前当前项目下能够通过模块路径找到的所有包，并生成依赖树。

npm doc <package name>可以打开该依赖包的官网，其实就是打开了package.json中的homepage。

## package.json文件
在运行npm init后会生成package.json文件，该文件用于记录项目中所用到的依赖以及项目的配置信息（比如名称、版本、许可证等）。npm install命令根据这个配置文件自动下载项目运行和开发所需要的依赖。
一个比较完整的package.json文件如下：
```
{
    "name": "project", //包名
    "version": "1.0.0", //版本号
    "author": "张三", //包的作者的名字
    "description": "第一个node.js程序",//包的描述
    "keywords":["node.js","javascript"], //关键字
    "repository": { //包代码存放的地方，可以是git或者svn
        "type": "git",
        "url": "https://path/to/url"
    },
    "license":"MIT",//开源许可证
    "engines": {"node": "0.10.x"},
    "bugs":{"url":"http://path/to/bug","email":"bug@example.com"},
    "contributors":[{"name":"李四","email":"lisi@example.com"}],
    "scripts": {
        "start": "node index.js"
    },
    "dependencies": {
        "express": "latest",
        "mongoose": "~3.8.3"
    },
    "devDependencies": {
        "grunt": "~0.4.1",
        "grunt-contrib-concat": "~0.3.0"
    }
}
```
1. 小标签1：如何为代码选择开源许可证，这是一个问题。
世界上的开源许可证，大概有上百种。很少有人搞得清楚它们的区别。即使在最流行的六种----GPL、BSD、MIT、Mozilla、Apache和LGPL----之中做选择，也很复杂。乌克兰程序员Paul Bagwell，画了一张分析图，说明应该怎么选择。只用两分钟，你就能搞清楚这六种许可证之间的最大区别。下面是阮一峰老师制作的中文版，请点击看大图。
![](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/license.png)
2. engines
```
可选字段。既可以指定node版本:
 { "engines" : {"node" : ">=0.10.3 <0.12" } }
也可以指定npm版本：
 { "engines" : {"npm" : "~1.0.20" } }
```

3. scripts：脚本说明对象。它主要被包管理器用来安装、编译、测试和卸载包.
示例如下:
```

"scripts":{

    “install”:"install.js",

    "test":"test.js"

}
```
4. main：模块引入方法require()在引入包时，会优先检查这个字段，并将其作为包中其余模块的入口，如果该字段不存在，则node会检查目录下的index.js，index.node，index.json作为默认入口。


### 使用淘宝 NPM 镜像
大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了：
```
cnpm install [name]
```

### NPM 常用命令
```
NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。

NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。

使用npm help <command>可查看某条命令的详细帮助，例如npm help install。

在package.json所在目录下使用npm install . -g可先在本地安装当前命令行程序，可用于发布前的本地测试。

使用npm update <package>可以把当前目录下node_modules子目录里边的对应模块更新至最新版本。

使用npm update <package> -g可以把全局安装的对应命令行程序更新至最新版。

使用npm cache clear可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。

使用npm unpublish <package>@<version>可以撤销发布自己发布过的某个版本代码。
```



原文：
https://segmentfault.com/a/1190000007624021
http://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html
https://www.runoob.com/nodejs/nodejs-npm.html
