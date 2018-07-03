title: node_module无法删除？
date: 2018/07/03
categories:
  - 十万个为什么
tags:
  - node
---

windows下 删除node_modules文件夹，解决目录层次太深删除报错的问题

解决方法：

使用npm中的插件rimraf，专门用于删除的模块插件
```
	1、安装：npm install -g rimraf（全局安装）

  2、使用：先定位目标文件夹的父级目录，然后命令行输入rimraf xxx（xxx为需要删除的文件夹名称）。
```

原文链接：http://blog.csdn.net/sensation_cyq/article/details/51657595

亲测可用~
