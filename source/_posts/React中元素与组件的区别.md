title: React中元素与组件的区别
date: 2018/07/16
categories:
  - 库/框架
toc: true
tags:
  - React
---
### React 元素
React 元素（React element），它是 React 中最小基本单位，我们可以使用 JSX 语法轻松地创建一个 React 元素:
```
const element = <div className="element">I'm element</div>
```
React 元素不是真实的 DOM 元素，**它仅仅是 js 的普通对象（plain objects）**，所以也没办法直接调用 DOM 原生的 API。
上面的 JSX 转译后的对象大概是这样的：
```
{
    _context: Object,
    _owner: null,
    key: null,
    props: {
    className: 'element'，
    children: 'I'm element'
  },
    ref: null,
    type: "div"
}
```

**只有在这个元素渲染被完成后，才能通过选择器的方式获取它对应的 DOM 元素。** 不过，按照 React 有限状态机的设计思想，应该使用状态和属性来表述组件，要尽量避免 DOM 操作，即便要进行 DOM 操作，也应该使用 React 提供的接口ref和getDOMNode()。一般使用 React 提供的接口就足以应付需要 DOM 操作的场景了，因此像 jQuery 强大的选择器在 React 中几乎没有用武之地了。

除了使用 JSX 语法，我们还可以使用 React.createElement() 和 React.cloneElement() 来构建 React 元素。

### React.createElement()
JSX 语法就是用React.createElement()来构建 React 元素的。它接受三个参数，第一个参数可以是一个标签名。如div、span，或者 React 组件。第二个参数为传入的属性。第三个以及之后的参数，皆作为组件的子组件。
```
React.createElement(
    type,
    [props],
    [...children]
)
```


原文：
https://segmentfault.com/a/1190000008587988
