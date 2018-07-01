title: React基础之 使用排坑日常
date: 2018/06/29
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

### Refs

React支持一个可以附加到任何组件的特殊属性ref。ref属性可以是一个字符串或一个回调函数。
当ref属性是一个回调函数时，函数接收底层DOM元素或类实例（取决于元素的类型）作为参数。这使你可以直接访问DOM元素或组件实例。



### 实际开发中遇到的问题

1. render()里面只能return一个JSX，

   因此在使用数组的.map()方法时，每次循环都要return一个JSX。

2. 在使用数组的.map()方法时，建议使用ES6的箭头函数，避免出现this指向的问题。

		oneArray.map((item,index)=>{
			return (
				<a onClick={this.play.bind(this)}>test</a>
			)
		})

3. 在写一个onClick的时候，如果这个function中没有用到this.state或者this.props时，

	建议不要使用this.test.bind(this)这种形式的写法，因为都要重新渲染组件，影响性能。

	常用的几种写法有:

		(1)没有入参时
			onClick={this.test}

		(2)有入参时
			onClick={this.test('1')}

		(3)语句很少时
			onClick={() => this.state.triggle = !this.state.triggle}

4. 父组件调用子组件的方法：
	[父组件调用子组件的方法](https://shengyur.github.io/2018/06/02/React%E5%9F%BA%E7%A1%80%E4%B9%8B%20%E7%88%B6%E7%BB%84%E4%BB%B6%E5%A6%82%E4%BD%95%E8%B0%83%E7%94%A8%E5%AD%90%E7%BB%84%E4%BB%B6%E4%B8%AD%E7%9A%84%E6%96%B9%E6%B3%95/)

5. es6中，寻找数组中是否包含某个元素，
	let arr = [1,2,3,-5]
	arr.find((n) => n<0)  //-5
	arr.findIndex((n) => n<0)  //3

	注意:indexOf方法无法识别数组的NaN成员，但是findIndex可以通过Object.is方法做到。
	[NaN].findIndex(y => Object.is(NaN,y))

6. 使用this.forceUpdate()来更新当前组件的render()方法。

7. 如果两个兄弟组件A和B，A想调用B组件的方法，必须通过两兄弟的父组件C来调用。


参考：
- [React中文文档](http://www.css88.com/react/docs/handling-events.html)
- [王老师的积累](https://github.com/wang-qingqing/accumulate/blob/master/%E6%A1%86%E6%9E%B6%E7%B1%BB/REACT/React%E5%BC%80%E5%8F%91%E4%B8%AD%E9%81%87%E5%88%B0%E7%9A%84%E9%97%AE%E9%A2%98.md)
