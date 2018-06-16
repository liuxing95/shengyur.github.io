title: React-Element中的StyleSheet.reset方法为何可以重置样式？
date: 2018/06/13
categories: 十万个为什么
tags:
  - Javascript
  - React

---

在React-Element的Select组件中，一上来就来了行看不懂的代码，
```
StyleSheet.reset(`
  .ishow-select-dropdown {
    position: absolute !important;
  }
`)
```
发现该方法可以实现，在当前页面插入带着样式的style标签，来实现对css的覆盖，心生疑虑，这到底是react的API还是es7黑魔法？

而后发现 Style只是从外部引入的一个模块，代码 动态添加了style节点

```
exports.reset = css => {
  const style = document.createElement('style'); //通过指定名称创建一个style元素

  style.type = 'text/css';
  //1.必需的 type 属性规定样式表的 MIME 类型。  2.type 属性指示 <style> 与 </style> 标签之间的内容。 3.值 "text/css" 指示内容是标准的 CSS。

  if (style.styleSheet){
    style.styleSheet.cssText = css;
  } else {
    style.appendChild(document.createTextNode(css));
  }
«»
  (document.head || document.getElementsByTagName('head')[0]).appendChild(style);
}
```

被这波原生js操作晃瞎了眼呀~只好再复习下原生用法

- [style 标签的 type 属性](http://www.w3school.com.cn/tags/att_style_type.asp)
