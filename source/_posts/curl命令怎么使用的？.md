title: curl命令是怎么使用的？
date: 2018/07/21
categories:
  - 十万个为什么
tags:
  - HTTP
---

在了解http请求的过程中，发现除了可以通过chrome控制台调试工具，也可以直接在mac(内置Linux)命令行中，使用curl命令,并且在很多命令行操作中，会返回curl的报错提示，已经错误码。

经了解，curl命令是一个利用URL规则在命令行下工作的文件传输工具。它支持文件的上传和下载，所以是 综合传输工具 ，但按传统，习惯称curl为下载工具。作为一款强力工具，curl支持包括HTTP、HTTPS、ftp等众多协议，还支持POST、cookies、认证、从指定偏移处下载部分文件、用户代理字符串、限速、文件大小、进度条等特征。做网页处理流程和数据检索自动化，curl可以祝一臂之力。


语法：

```
curl(选项)(参数)
```

功能强大，不过目前比较常用的是 只打印响应头部信息 的功能，**通过-I或者-head可以只打印出HTTP头部信息**,来快速分析常见的http请求头

![curl --head xxx](https://raw.githubusercontent.com/shengyur/Images/master/curl.jpg)


原文：
[curl命令们](http://man.linuxde.net/curl)
[curl文档](https://curl.haxx.se/docs/manpage.html)
