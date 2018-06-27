title: Array、Object常用方法（ES3、ES5、ES6）
date: 2018/06/25
categories: 前端基础
toc: true
tags:
  - ES6
  - ES5
---

使用场景：
使用react、vue等数据驱动的框架时，dom操作基本不用了，更多的是修改数据来驱动UI渲染。
所以总结一下常用的方法，以提高coding效率~

## 数组方法

### 数组元素的添加和删除
1. 为新索引赋值
```
arr[0]="zero"
```
2. 使用arr.push()在数组末尾增加一个或多个元素：
```
arr.push("zero")
```
3. 使用arr.pop()来从末尾推出一个值，并返回被推出的值

4. 使用arr.shift()方法，从数组头部删除一个元素,返回被删除的元素，整个数组的index少1

5. 使用arr.unshift()方法，从数组头部推入一个元素，返回新数组的长度

6. 使用delete运算符来删除数组元素(原数组会变成稀疏数组)
```
a=[1,2,3]
delete a[1];
1 in a   //false
a.length //3 delete操作并不会影响数组长度  
```
7. 设置length属性为一个新的期望长度来删除数组 **末尾** 的元素arr

8. splice()方法用来插入、删除、替换元素

### 数组遍历

1. for循环
  Object.keys(arr)可以获得数组的下标

2. for in (可以枚举继承的属性名，如添加到Array.property中的方法)
```
  for(var i in arr){
    if(!a.hasOwnProperty(i)) continue;//跳过继承的属性
    //循环体
  }

  或

  for(var i in arr){
    if(String(Math.floor(Math.abs(Number(i))))!==i) continue;//跳过不是非负整数的i
  }
```
3. es5中提供了forEach()方法

### 数组方法

(ES3)
1. Array.join()方法
将数组中的所有元素转化为字符串并拼接在一起，
```
var arr=[1,2,3,4];
arr.join(",")   //"1,2,3,4"
```
Array.join()方法是String.split()方法的逆向操作，后者是把字符串分割成若干块来创建一个数组。

2. Array.reverse() （修改原数组）
不通过用重新排列的元素创建新数组，而是在原先的数组中重新排列他们








参考：
 https://segmentfault.com/a/1190000014100810?utm_source=index-hottest
