title: 为什么git经常无法提交？
date: 2018/07/26
categories:
  - 十万个为什么
tags:
  - git
---


报错1：

```Linux
fatal: unable to access 'https://github.com/shengyur/shengyur.github.io.git/': Could not resolve host: github.com
```

报错2：

```Linux
 rewrite categories/Questions/index.html (80%)
 rewrite index.html (77%)
 rewrite page/3/index.html (61%)
 rewrite tags/git/index.html (78%)
```
push 时，进度疯狂卡住，太焦躁了

```Linux
fatal: unable to access 'https://github.com/shengyur/shengyur.github.io.git/': Server aborted the SSL handshake
```

报错3：

```Linux
Could not resolve host: github.com
```

报错原因：

域名解析不了，推测是网络问题,检查设备，发现vpn和翻墙工具都开好了，难不成是网速问题。。。

然后把电脑连上手机热点，三个报错就都不见了 == ，吐槽下南京电信，真是辣鸡





参考：
- [github 创建远程分支以及远程分支无法删除的问题解决](https://blog.csdn.net/u014182411/article/details/74011901)
