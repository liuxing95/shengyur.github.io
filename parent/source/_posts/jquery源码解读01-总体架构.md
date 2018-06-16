title: jquery源码解析01——总体架构
date: 2018/4/27
categories: 源码浅析
tags:
  - jQuery源码
---

## jquery1.7.1源码的总体结构：
```
(function( window , undefined ){
//构造jQuery对象
var  jQuery =  (function(){
var jQuery =  function (  selector,  context ){
return new jQuery.fn.init(selector,context,rootjQuery);
}
return jQuery;
})();
//工具方法：Utilities
//回调函数列表：Callbacks  Object
//异步队列：Deferred  Object
//浏览器功能测试：Support
//数据缓存：DataS
//队列：Queue
//属性操作：Attributes
//事件系统：Events
//选择器：Sizzle
//DOM遍历：Traversing
//DOM操作：Manipulation
//样式操作 css  (计算样式、内联样式)
//异步请求：Ajax
//动画： Effects
//坐标：Offset、尺寸 Dimensions
window.jQuery =  window.$  =  jQuery;
})(window);
```

### 为什么要创建一个自调用匿名函数？
因为js只有没有函数作用域，所以使用匿名函数自调来创建特殊的函数作用域，这个作用域中的代码不会和已有的同名函数、方法和变量以及第三方库冲突

### 匿名函数自调有几种不同的写法？
```
1.（function(){
//...
})();
```
```
2: (function(){
//...
}());
```
```
3:	!function(){
//...
}();
```

### 为什么匿名函数自调 要在在前面加！？
匿名函数自调叫做**立即调用的函数表达式**更为贴切,直接执行,会报错(语法错误SyntaxError)
语法错误的两种原因：
- function (){ }()
期望是立即调用一个匿名函数表达式，结果是进行了函数声明，函数声明必须要有标识符做为函数名称
-  function g(){ }()
期望是立即调用一个具名函数表达式，结果是声明了函数 g。末尾的括号作为分组运算符，必须要提供表达式做为参数。
所以那些匿名函数附近使用括号或一些一元运算符的惯用法，就是来引导解析器，指明运算符附近是一个表达式。
执行匿名函数可以通过+，-，！，（） 这样的形式来转化为函数表达式，就可以通过（）来运行了。
<br>
注意：
圆括号运算符也叫分组运算符，它有两种用法：如果表达式放在圆括号中，作用是求值；如果跟在函数后面，作用是调用函数

### 为什么要自调用匿名函数设置参数window，并传入window对象？
- 通过传入window,可以把window变成局部变量，这样在jquery的代码快中访问window时，不需要将作用域链回退到顶层作用域，
从而可以更快的访问window对象
- 将window对象作为参数传入，可以在压缩代码时进行优化，在压缩文件中可以发现
(function(a,b){.......})(window)参数window被压缩成a,参数undefined被压缩成b

### 为什么要自调用匿名函数并设置参数undefined？
- undefined 是window对象的一个属性，通过把undefined当成局部变量使用，但是又不传入任何值，所以undefined的值 就是 “undefined”  可以缩短查找undefined的作用域链
- 在压缩代码时可以进行优化
- 重要：通过这个方法可以确保参数undefined的值是undefined，因为undefined有可能被重写为新的值


### 为什么匿名函数最后或者最开始要加分号？
- 因为如果自调用匿名函数（立即调用的函数表达）的前一行代码,没有加分号结尾,那么匿名函数的第一个括号会被认为是函数调用
