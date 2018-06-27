title: React基础之 使用排坑日常
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

### Props 是只读的,声明组件的函数必须是纯函数

无论你用函数或类的方法来声明组件, 它都无法修改其自身 props. 思考下列 sum (求和)函数:
```
function sum(a, b) {
  return a + b;
}
```
这种函数称为 **“纯函数”** ，因为它们不会试图改变它们的输入，并且对于同样的输入,始终可以得到相同的结果。
反之， 以下是非纯函数， 因为它改变了自身的输入值：
```
function withdraw(account, amount) {
  account.total -= amount;
}
```
所有 React 组件都必须是纯函数，并禁止修改其自身 props 。

### 单项数据流(数据向下流动）

state(状态)经常被称为 本地状态 或 封装状态，是因为它不能 被拥有并设置它的组件以外的任何组件 访问。
一个组件可以选择将state向下传递，作为其子组件的props属性：
```
<h2>it is {this.state.date.toLocalTimeString()}</h2>
```
同样适用于用户定义组件：
```
<Formatte date={this.state.date}>
```
Formatte组件通过props属性接受了date的值，但它仍任然不能获知该值是来自于哪。

这通常称为"从上而下"，或者单项数据流。任何state始终由某个特定组件所有，并且从该state导出的任何数据 或 UI只能影响树种 "下方"的组件。


参考：
- [React中文文档](http://www.css88.com/react/docs/handling-events.html)
