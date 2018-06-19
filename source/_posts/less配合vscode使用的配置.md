title: less配合vscode使用的配置
date: 2018/05/05
categories: 效率工具
tags:
  - Less
  - vscode
---

使用场景：写jquery老项目懒得配置css预编译的情况下，安装即用，提升效率。

1. 安装 easy less

2. 安装Preview on web server

3. 在.less 文件最后一行敲下回车，就可以实现编译main.less到main.css了。

如果不生效，就试试下面这个：

1. 安装less转码器
```
npm install -g node-sass less
```
2. 在css文件下建个less文件（后缀名为less）

3. Ctrl + Shift + P（配置任务运行器）
选择Others
替换内容为：
```
  1. // Less configuration
  2. {
  3. "version": "0.1.0",
  4. "command": "lessc",
  5. "isShellCommand": true,
  6. "args": ["css/名字.less", "css/名字.css"]
  7. }
  8. }
```
