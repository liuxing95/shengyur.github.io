title: CSS效率工具之 Sass的常见用法
date: 2018/05/25
categories: 效率工具
tags:
  - Sass
---
<figure>
<img src="https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/sass.jpg">
</figure>
### 一 使用变量
1. 可以使用变量：
声明变量的方式：使用$符号，$highlight-color  $sidebar-width

2. 变量存在作用域
作用域使用方式：类似ES6的块级作用域，使用{}区分作用域
  <!--more-->

```
$nav-color: #F90;
nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}

//编译后

nav {
  width: 100px;
  color: #F90;
}
```

3. 变量如何命名更好
sass的变量名可以包括中划线和下划线，如$highlight-color（更普遍）、$highlight_color ，在sass中，这两种写法的内容是互通的，比如，对sass来说$link-color和$link_color其实指向的是同一个变量，但是在sass中纯css部分不互通，比如类名、ID或属性名。

```
$link-color: blue;
a {
  color: $link_color;
}

//编译后

a {
  color: blue;
}
```

### 二 嵌套CSS
1. 可以避免书写重复的长串选择器

```
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

例外情况：
比如你想要在嵌套的选择器 里边立刻应用一个类似于：hover的伪类。为了解决这种以及其他情况，sass提供了一个特殊结构 &。

2. 父选择器的标识符&

```
article a {
  color: blue;
  &:hover { color: red }
}
```

当包含父选择器标识符的嵌套规则被打开时，它不会像后代选择器那样进行拼接，而是&被父选择器直接替换：

```
article a { color: blue }
article a:hover { color: red }
```

3.  群组选择器的嵌套(减少重复敲写)
.container h1, .container h2, .container h3 { margin-bottom: .8em }
可以写成：

```
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
```

4. 子组合选择器和同层组合选择器：>、+和~ 都支持使用

&gt;:子组合选择器,用于选择一个元素的直接子元素
+:同层相邻组合选择器+选择header元素后紧跟的p元素
~:同层全体组合选择器~，选择所有跟在article后的同层article元素，不管它们之间隔了多少其他元素

```
article ~ article { border-top: 1px dashed #ccc }
```

5. 属性也可以嵌套

```
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
```

嵌套属性的规则：
把属性名从中划线-的地方断开，在根属性后边添加一个冒号:，紧跟一个{ }块，把子属性部分写在这个{ }块中。

```
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

### 三、导入SASS文件
css原生的特性@import规则，允许在一个css文件中导入其他css文件，后果是只有执行到@import时，浏览器才会去下载其他css文件，这导致页面加载起来特别慢。

sass的@import规则，在生成css文件时，就会把相关样式导入进来。而且导入时，可以省略后缀的书写
如@import"sidebar";这条命令将把sidebar.scss文件中所有样式添加到当前样式表中。

常用的就以上几点了，记录一下方便以后翻阅。

参考：https://www.sass.hk/guide/
