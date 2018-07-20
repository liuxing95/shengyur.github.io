title: React 深入之 diff算法
date: 2018/07/19
categories:
  - 库/框架
toc: true
tags:
  - React
  - diff算法
---

### React diff算法优点在哪？
计算一棵树形结构转换成另外一棵树结构的最少操作，是一个复杂且值得研究的问题。传统diff算法通过循环递归对子节点进行一次对比，效率低下，算法复杂度达到O(n<sup>3</sup>),其中n是树节点的总数。计算量非常大，所以想要将diff思想引入Virtual Dom，就要设计一种稳定、高效的diff算法。


### 每种UI场景中，diff算法是如何进行操作的？
假设场景：
![虚拟dom是如何工作的](https://raw.githubusercontent.com/shengyur/Images/master/react%20diff%20/diff1.jpg)
当dom从第一个场景变化为第二个场景时，程序是如何自动diff的？


采取的是 **广度优先分层比较**：
拿到前后两个状态的dom树之后，会对他们从上而下进行一层一层的比较，比如图中A和B的位置发生变化，A的子节点类型发生变化等
1. 从根节点进行比较
![从根节点进行比较](https://raw.githubusercontent.com/shengyur/Images/master/react%20diff%20/diff3.jpg)
发现两个节点是一样的，所以不做任何修改。

2. 属性以及顺序发生变化
![属性以及顺序发生变化](https://raw.githubusercontent.com/shengyur/Images/master/react%20diff%20/diff4.jpg)
程序需要知道A和B的唯一标识，来判断他们是否发生改变，检测到修改后，调换A和B的位置

3. 节点类型发生变化
![节点类型发生变化](https://raw.githubusercontent.com/shengyur/Images/master/react%20diff%20/diff5.jpg)
A下面的节点从F节点变成G节点，节点类型发生变化时，React会直接把F节点删掉，创建新节点然后append到A上面，而不会去管F节点是否被其他地方引用到。
只会简单的把F删掉，换成G

4. 节点跨层移动（react核心优化的点）
![节点跨层移动](https://raw.githubusercontent.com/shengyur/Images/master/react%20diff%20/6.jpg)

从图上可以看到B与原来的子元素D中间加了一个元素，然而react并不会这样操作，diff算法会直接从B节点下面把D节点已经D的子元素全部删掉，
比对到第四层的时候，会再创建一个全新的D节点添加到树上去

React由于采用了分层比较的方法，把算法复杂度从O(n^3)降低到了O(n)，那么该策略是否会在跟新时有问题呢？

事实证明并不会，因为大多数情况下，我们的dom结构是相对固定的，一般都是dom节点的位置或者属性发生变化，或者新建、删除节点等，真实情况下很少会发生跨层移动的场景，所以舍弃这一层比较，会极大提升react性能而无太大影响


### 虚拟DOM基于的两个假设
1. 组件的 DOM 结构是相对稳定的
2. 类型相同的兄弟节点可以被唯一标识(key属性不仅被用来消除warning,更是提高性能的一种方式)


参考：
1. https://doc.react-china.org/docs/reconciliation.html
2. 《深入React技术栈》
