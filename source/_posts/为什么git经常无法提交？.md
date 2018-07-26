title: 为什么git经常无法提交？
date: 2018/07/26
categories:
  - 十万个为什么
tags:
  - git
---



![南京](http://i2.bvimg.com/655421/2ab335338dece036.jpg)

最近一周github总是提交不上去，并且打开github页面非常卡，一git push就报错，百度一堆答案众说纷纭，气愤的很，难不成是我南京进梅雨季节了网速不好么  谜团在今晚终于解开了 ==

报错1：

```Linux
Could not resolve host: github.com
```

报错原因：

域名解析不了，推测是网络问题

报错2：

```Linux
fatal: The remote end hung up unexpectedly
```

报错3：

```Linux
 rewrite categories/Questions/index.html (80%)
 rewrite index.html (77%)
 rewrite page/3/index.html (61%)
 rewrite tags/git/index.html (78%)
```
push 时，进度疯狂卡住，太焦躁了





参考：
- [github 创建远程分支以及远程分支无法删除的问题解决](https://blog.csdn.net/u014182411/article/details/74011901)
