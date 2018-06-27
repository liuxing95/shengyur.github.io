title: React基础之 排坑日常
date: 2018/06/27
categories:
  - 库/框架
toc: true
tags:
  - React
---


### state(状态) 更新可能是异步的


React 为了优化性能，有可能会将多个 setState() 调用合并为一次更新。

**因为 this.props 和 this.state 可能是异步更新的，你不能依赖他们的值计算下一个state(状态)。**

例如, 以下代码可能导致 counter(计数器)更新失败：
```
// 错误
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
要弥补这个问题，需要使用另一种 setState() 的形式，**它接受一个函数,而不是一个对象。这个函数将接收前一个状态作为第一个参数，应用更新时的 props 作为第二个参数：**
```
// 正确
this.setState((prevState, props) => ({
  //prevState:更新之前的状态state值
  //props:更新之前的props
  counter: prevState.counter + props.increment
}));
```
我们在上面使用了一个箭头函数，但是也可以使用一个常规的函数：
```
// 正确
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```
