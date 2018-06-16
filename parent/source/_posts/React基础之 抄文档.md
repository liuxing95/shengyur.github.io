title: React基础之 抄文档
date: 2018/06/11
categories: 库/框架
tags:
  - React
---
### React 组件 API
#### 强制更新：forceUpdate()
```
component.forceUpdate(callback)
```

“By default, when your component’s state or props change, your component will re-render. If your render() method depends on some other data, you can tell React that the component needs re-rendering by calling forceUpdate().

Calling forceUpdate() will cause render() to be called on the component, skipping shouldComponentUpdate(). This will trigger the normal lifecycle methods for child components, including the shouldComponentUpdate() method of each child. React will still only update the DOM if the markup changes.

Normally you should try to avoid all uses of forceUpdate() and only read from this.props and this.state in render().”

默认情况下，当组件的state或者props属性变化时，组件将会重新渲染。然而，如果你的render()方法依赖一些别的数据。你可以通过调用forceUpdate()来让组件实现重新渲染。
调用forceUpdate()，将会导致组件直接跳过shouldComponentUpdate()方法，去执行render()方法，这会触发子组件们正常的生命周期方法，包括每个子组件的shouldComponentUpdate()方法。但是，组件重新渲染时，依然会读取this.props和this.state，如果状态没有改变，那么React只会更新DOM。
**一般来说，应该尽量避免使用forceUpdate()，而仅从this.props和this.state中读取状态并由React触发render()调用。**

- 使用场景：
forceUpdate就是重新render。有些变量不在state上，但是你又想达到这个变量更新的时候，刷新render；或者state里的某个变量层次太深，更新的时候没有自动触发render。这些时候都可以手动调用forceUpdate自动触发render


原文：
- http://www.runoob.com/react/react-component-api.html
- https://reactjs.org/docs/react-component.html#forceupdate
