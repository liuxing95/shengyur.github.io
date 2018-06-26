title: ES3、ES5、ES6常用操作数组对象方法
date: 2018/06/25
categories: 前端基础
tags:
  - ES6
---

使用场景：
使用react、vue等数据驱动的框架时，dom操作基本不用了，更多的是修改数据来驱动UI渲染。
所以总结一下常用的方法，以提高coding效率~

## 数组方法

### Array 对象属性
- constructor 返回对创建此对象的数组函数的引用。
- length 设置或返回数组中元素的数目。
- prototype 使您有能力向对象添加属性和方法。

<!--more-->

### 传统Array 对象方法
- toSource() 返回该对象的源代码。
- toString() 把数组转换为字符串，并返回结果。
- toLocaleString() 把数组转换为本地数组，并返回结果。
- valueOf() 返回数组对象的原始值

### 修改原数组
- push() 向数组的末尾添加一个或更多元素，并返回新的长度。
- unshift() 向数组的开头添加一个或更多元素，并返回新的长度。
- pop() 删除并返回数组的最后一个元素
- shift() 删除并返回数组的第一个元素
- sort() 对数组的元素进行排序
- reverse() 颠倒数组中元素的顺序。
- splice() 删除元素，并向数组添加新元素。


参考：
 https://segmentfault.com/a/1190000014100810?utm_source=index-hottest
