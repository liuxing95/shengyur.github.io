title: ES6之扩展运算符与应用
date: 2018/07/20
categories: 前端基础
tags:
  - ES6
---

在使用react的过程中，会有如下写法
```
const data={name:"ssdfin"，value:1}
const component = <Component {...data}>
```
发现对 ... 三个点理解不是很深，所以单独记录下 ES6 rest/spread 特性

### 扩展运算符(spread)

1. 场景1：使用在数组之前。
作用：将一个数组转为用逗号分隔的参数序列

```javascript
function foo(x, y, z){
    console.log(x, y, z)
}
foo.apply(null, [1, 2, 3])　　//在ES6之前我们这样使用数组作为函数参数调用。
foo(...[1, 2, 3])　　//此处...[1, 2, 3]就被展开为用逗号隔开的1, 2, 3参数序列
```

　当运算符"..."用在数组之前时，数组会被转为用逗号分隔的参数序列。

2. 场景2：替代apply()方法

```JavaScript
// ES5的 写法
var arr1 = [0, 1, 2];
var arr2 = [3, 4, 5];
Array.prototype.push.apply(arr1, arr2);　　//push方法参数不能为数组，ES5需要借助apply()方法实现。

// ES6 的写法
let arr1 = [0, 1, 2];
let arr2 = [3, 4, 5];
arr1.push(...arr2);　　　　　　　　　　　　　　//ES6中借助扩展运算符直接将数组转为了参数序列。
```

3. 场景3：替代数组的concat()方法

```javascript
let a = [2, 3, 4]
let b = [1, ...a, 5]    //此处a数组被展开为2, 3, 4
console.log(b)          //结果为[1, 2, 3, 4, 5]
```
上面的用法基本上替代了concat(..)，这里的行为就像[1].concat(a, [5])

注意：扩展运算符后如果是空数组，不会产生任何效果

```javascript
[...[], 1]
// [1]
```


### 收集运算符(rest)

作用：收集剩余的参数转为一个数组。

　　场景：在函数参数之前使用。

1. 场景1：函数参数之前
```javascript


function foo(x, y, ...z){   //z表示把剩余的参数收集到一起组成一个名叫z的数组。
    console.log(x, y, z)
}
foo(1, 2, 3, 4, 5)          //x赋值1，y赋值2，z中赋值[3, 4, 5]数组
```
2. 场景2：结构赋值

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];　　//此处'...'作为rest收集运算符使用
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

### 总结：
如何判断ES6中的运算符是扩展运算符(spread)还是收集运算符(rest)，主要取决于其作用的位置。

1.数组之前，作为扩展运算符使用，将数组转为逗号分隔的参数序列。

2.函数形参中，收集传入的参数为数组。

3.解构赋值中，收集对应的数据为数组。

原文：
1. [ES6之扩展运算符与应用(数组篇)](https://www.jianshu.com/p/41f499fa0e7b)
2. [ES6之扩展运算符与应用(对象篇)](https://www.jianshu.com/p/35f9efe95fff)
3. https://www.cnblogs.com/diweikang/p/8976854.html
