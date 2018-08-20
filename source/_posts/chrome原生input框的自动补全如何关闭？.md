title: chrome原生input框的自动补全如何关闭？
date: 2018/08/20
categories:
  - 十万个为什么
tags:
  - HTML
---

在input框中进行第二次输入时，会自动出现以前填写过的信息下拉autocomplete，然而用在 **输入验证码的输入框的场景** 下时，由于验证码两次肯定不会重复，
应该关掉改功能。
<!--more-->

可以使用 autocomplete = "off" 来进行取消。

提示项被选中时，背景会变成黄色，使用
```CSS
input:-webkit-autofill{
 -webkit-box-shadow:inset 0 0 0 1000px #fff;
background-color:transpatent;
}
```
可以修成白色。

参考：
http://www.w3school.com.cn/tiy/t.asp?f=html5_input_autocomplete
