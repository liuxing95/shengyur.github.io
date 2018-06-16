title: vscode 之多设备配置同步
date: 2018/06/01
categories: 效率工具
tags:
  - vscode
---

#### 背景：
多台电脑同时使用的情况下，每次换电脑都重新配置一次就太麻烦了。得同步一把啊~

#### 解决：
实际上，Atom 和 VSC 都提供了 Sync 插件，比如 VSC 的 Settings Sync，可以轻松将 IDE 的所有配置一键备份到 github 上，也可以将 github 上的配置一键拉取下来，然后重启 IDE 便可以共享同样的配置了。
<!--more-->

这个配置并不复杂，下面来简单介绍下它的使用：
- 在左侧的 sidebar 选中最后一个，搜索 Sync，不出意外，你会从前几个中找到下载量很高的那个 Settings Sync；
- 安装后，摁下 Command + Shift + P 打开控制面板，搜索 Sync，选择 update/upload 可以上传你的配置，选择 Download 会下载远程配置；
- 如果你之前没有使用过 Sync，在上传配置的时候，会让你在 Github 上创建一个授权码，允许 IDE 在你的 gist 中创建资源；下载远程配置，你可以直接将 gist 的 id 填入；
- 下载后等待安装，然后重启即可

顺便纪念下2018儿童节的第一分钟，以及那些年我们吹过的牛逼
![我们的征途，是星辰大海](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/lufi.jpg)
原文：
https://www.barretlee.com/blog/2017/04/21/something-about-vsc/
