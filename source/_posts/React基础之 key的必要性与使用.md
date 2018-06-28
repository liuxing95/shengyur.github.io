title: React基础之 React基础之 key的必要性与使用
date: 2018/06/28
categories:
  - 库/框架
toc: true
tags:
  - React
---

#### 参考资料：
 https://segmentfault.com/a/1190000009149186

#### 总结
1. **数组数据变更时，不要使用index作为key。**

2. key的值要保持稳定且唯一，不能使用random来生成key的值。

  new Date().getTime()好像不太好，但是感觉可以解决问题呢。==

3. key属性是添加到自定义的子组件上，而不是子组件内部的顶层的组件上。

    <MyComponent key={{item.key}}/>
<!--more-->
4. key值的唯一是有范围的，即在数组生成的同级同类型的组件上要保持唯一，而不是所有组件的key都要保持唯一。


5. 不仅仅在数组生成组件上，其他地方也可以使用key，

    主要是react利用key来区分组件的，相同的key表示同一个组件，react不会重新销毁创建组件实例，只可能更新；key不同，react会销毁已有的组件实例，重新创建组件新的实例
#### 具体内容
1. key的作用

        react利用key来识别组件，它是一种身份标识标识。

        每个key对应一个组件，相同的key react认为是同一个组件，这样后续相同的key对应组件都不会被创建。

2. 有了key属性后，就可以与组件建立了一种对应关系，react根据key来决定是销毁重新创建组件还是更新组件。

        A.key相同，若组件属性有所变化，则react只更新组件对应的属性；没有变化则不更新。

        B.key值不同，则react先销毁该组件(有状态组件的componentWillUnmount会执行)，然后重新创建该组件（有状态组件的constructor和componentWillUnmount都会执行）

3. key不是用来提升react的性能的，不过用好key对性能是有帮助的。

4. 使用最多的场景

        A.由数组动态创建的子组件的情况，需要为每个子组件添加唯一的key属性值。

        B.为一个有复杂繁琐逻辑的组件添加key后，后续操作可以改变该组件的key属性值，从而达到先销毁之前的组件，再重新创建该组件。

5. index作为key是一种反模式

        A.若数组的内容只是作为纯展示，而不涉及到数组的动态变更，其实是可以使用index作为key的。

        B.若涉及到数组的动态变更，例如数组新增元素、删除元素或者重新排序等，这时index作为key会导致展示错误的数据。

6. 动态数据使用index作为key的问题：

    具体更新过程我们拿key=0的元素来说明， 数组重新排序后：

        A.组件重新render得到新的虚拟dom；

        B.新老两个虚拟dom进行diff，新老版的都有key=0的组件，react认为同一个组件，则只可能更新组件；

        C.然后比较其children，发现内容的文本内容不同（由a--->c)，而input组件并没有变化，这时触发组件的componentWillReceiveProps方法，从而更新其子组件文本内容;

        D.因为组件的children中input组件没有变化，其又与父组件传入的任props没有关联，所以input组件不会更新(即其componentWillReceiveProps方法不会被执行)，导致用户输入的值不会变化。
