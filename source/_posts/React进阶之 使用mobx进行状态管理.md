title: React进阶之 使用mobx进行状态管理
date: 2018/06/11
categories: 库/框架
toc: true
tags:
  - React
---



### mobx解决了什么问题
react 关注的状态(state)到视图(view)的问题。而 mobx 关注的是状态仓库（store）到的状态(state)的问题。
任何源自应用状态的东西都应该自动地获得。

#### 与react的关系
react提供了优化UI渲染的机制，这种机制就是通过使用虚拟DOM来减少昂贵的DOM变化的数量。mobx提供了优化应用状态与react组件同步的机制，这种机制就是使用响应式虚拟依赖状态图标，它只有在真正需要的时候才更新并且永远保持是最新的。

#### 核心概念
mobx 最最核心的概念只有2个。 @observable 和 @observer ，它们分别对应的是被观察者和观察者。这是大家常见的观察者模式，不过这里使用了，ES7 中的 装饰器。

##### Observable state（可观察的状态）

 通过使用@observable装饰器来给现有的数据结构（如对象、数组和类实例）添加可观察的功能。
 注：@observable是es.next的写法，在es5中，使用extendObservable()来实现。

##### Computed values（计算值）

 定义在相关数据发生变化时自动更新的值，可通过@computed 装饰器或者利用(extend)Observable时调用的getter/setter函数来进行使用。

##### Reactions（反应）
- reactions在响应式编程和命令式编程之间建立沟通的桥梁。

- reactions和计算值很像，但它不是产生一个新的值，而是会产生一些副作用，比如打印到控制台、网络请求、递增地更新react组件数以修补dom等。

- observer()会将组件转换为它们需要渲染的数据的衍生。
  import {observer} from 'mobx-react'
- 自定义reactions：
  使用autorun、reaction、when函数即可简单的创建自定义reactions，用来满足具体场景。
##### Actions(动作)
- 状态应该以某种状态来更新。

#### mobx会对什么作出响应？

会对在执行跟踪函数期间读取的任何现有的可观察属性做出反应。

#### mobx的优点：（简单且可扩展）

A.使用类和真正的引用 B.保证参照完整性 C.更简单的actions更便于维护 D.细粒度的可观测性是高效的 E.易操作性

#### mobx要点 将一个应用变成响应式的步骤
(1)定义状态并使其可观察

```
import {observable} from 'mobx';
var appState = observable({ timer: 0 })
```

(2)创建视图以响应状态的变化
```
import {observer} from 'mobx-react';

 @observer class TimerView extends React.Component{
    render(){
      return (
        Seconds passed: {this.props.appState.timer}
        )
    }

     onReset(){
         this.props.appState.resetTimer();
     }
 }

 ReactDOM.render(<TimerView appState={appState} />,document.body);
 ```
(3)更新状态（mobx帮助你以一种简单直观的方式来完成工作） appState.resetTimer = action(function reset(){ appState.timer = 0; })
```
 setInterval(action(function tick(){
     appState.timer += 1;
 ),1000)
 ```
#### 概念

1. State(状态)

 状态是驱动应用的数据。
2. Dervations(衍生)

- 任何源自状态并且不会再有任何进一步的相互作用的东西就是衍生。
- 衍生的存在形式：用户界面、衍生数据、后端集成
- mobx区分了两种类型的衍生：Computed values 和 Reactions
- 黄金法则：如果想创建一个基于当前状态的值时，使用computed
3. Actions(动作)

 动作是任一一段可以改变状态的代码。
#### 原则

- mobx支持单向数据流，也就是动作改变状态，而状态的改变会更新所有受影响的视图。

 Action -> State -> Views
- 当状态改变时，所有衍生都会进行原子级的自动更新。因此永远不可能观察到中间值。

- 所有衍生默认都是同步更新。这意味着例如动作可以在改变状态之后直接可以安全地检查计算值。

- 计算值是延迟更新的。任何不在使用状态的计算值将不会更新，直到需要它进行副作用（I/O）操作时。如果视图不再使用，那么他会自动被垃圾回收。

- 所有的计算值都应该是纯净的。它们不应该用来改变状态。

### 实际开发中遇到的问题及解决过程

1.  @observable 数组

 A.将数组变成可观察的对象时，map、forEach、indexOf、find等原生数组可用的方法，仍可以使用。

 B.在控制台打印这个数组时，已经变成了ObservableArray,并不是Array对象了。

 C.可以使用xxx.slice()将其变成原生数组。

原文：
- [JS 装饰器（Decorator）场景实战](https://juejin.im/post/59f1c484f265da431c6f8940)
- [王老师的积累](https://github.com/wang-qingqing/accumulate/blob/f5779c2a734420200a34531d2ad7bdcfd91bd0ec/%E6%A1%86%E6%9E%B6%E7%B1%BB/REACT/React%E7%8A%B6%E6%80%81%E7%AE%A1%E7%90%86/%E5%85%B3%E4%BA%8EMobx.md)

- [mobx中文文档](http://cn.mobx.js.org/)

- [mobx todolist](https://codesandbox.io/s/2vmzpM0wK)

- [mobx 在ReactJS项目中的运用](https://blog.csdn.net/u012125579/article/details/69400169)
