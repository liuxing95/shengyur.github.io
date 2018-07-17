title: React基础之 组件生命周期
date: 2018/07/09
categories: 库/框架
tags:
  - React
---
![](https://raw.githubusercontent.com/shengyur/Images/master/lifecycle.jpg)

<!--more-->
### constructor
1. 用于初始化内部状态，很少使用
2. 唯一可以直接修改state的地方


### getDerivedStateFromProps （react 16.3）
1. 当state需要从props初始化时，使用
2. **尽量不要使用，维护两者状态一致性会增加复杂度**
3. 每次render都会调用
运用场景：表单控件获取默认值

### componentDidMount
1. ui渲染完之后调用
2. 只执行一次
运用场景：获取外部资源

### componentWillUnmount
1. 组件移除时被调用
运用场景：资源释放

### getSnapshotBeforeUpdate (react 16.3）
1. 在页面render之前调用，state已跟新
运用场景：获取render之前的dom状态
getSnapshotBeforeUpdate()在最新的渲染输出提交给DOM前将会立即调用。它让你的组件能在当前的值可能要改变前获得它们。这一生命周期返回的任何值将会 作为参数被传递给componentDidUpdate()。

### componentDidUpdate
1. 每次UI跟新时被调用
运用场景：页面需要根据props变化重新获取数据

### shouldComponentUpdate
1. 决定 Virtual Dom是否要重绘
2. 一般不需要手动调用，可以使用 PureComponent 自动实现
运用场景：性能优化

[点击查看demo](https://github.com/shengyur/react-demo-code-16.3-)







原文：
- 图片来自 http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/
- https://www.zhihu.com/question/278328905/answer/399344422
- react生命周期 https://www.cnblogs.com/yangzhou33/p/8799278.html
