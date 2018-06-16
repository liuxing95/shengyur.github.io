title: git踩坑日常全记录
date: 2018/05/26
categories: 效率工具
tags:
  - git
---

在工作中使用的代码管理工具之前一直是svn，最近接触了几个使用git管理代码的项目，拉个代码拉了半天，十分生气，记录下踩坑日常，避免重蹈覆辙~
<!--more-->

### 权限已经开好，但是git clone提示找不到该路径???
1. 首先，确定权限已经是开好的，没问题
2. 回忆了一下公司的代码仓库，猜测可能是用户名与自己本地默认的自己github的账号密码不一样
3. 查看当前用户的配置
```
git config --global  --list
```
4. 发现显示的用户名果然与公司的域账号用户名密码不一致，那么问题来了，每次拉代码也没让输用户名密码，怕不是默认记住密码了？？
```
git config --system --list
```
- 查看系统config文件中，对是否记住密码的配置，果然
```
credential helper== credential helper
```
5. 清除保存的用户名密码
```
git credential-manager delete

或者

git credential-manager remove [--path <installion_path>] [--passive] [--force]   //停止使用管理工具[中括号选填]
```
6. 重新拉取代码，提示输入用户名密码，切换账户后问题解决

参考：
https://segmentfault.com/q/1010000007962678
