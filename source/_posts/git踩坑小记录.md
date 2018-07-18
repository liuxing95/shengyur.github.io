title: git菜鸟踩坑日常
date: 2018/06/20
categories: 效率工具
tags:
  - git
---

在工作中使用的代码管理工具之前一直是svn，最近接触了几个使用git管理代码的项目，记录下踩坑日常，避免重蹈覆辙~
<!--more-->

### 权限已经开，但是git clone提示找不到该路径?
1. 首先，确定权限已经是开好
2. 回忆了一下公司的代码仓库，猜测可能是用户名与自己本地默认的自己github的账号密码不一样
3. 查看当前用户的配置
```
git config --global  --list
```
4. 发现显示的用户名果然与公司的域账号用户名密码不一致，每次拉代码也没让输用户名密码，怕不是默认记住密码了？？?
```
git config --system --list
```
- 查看系统config文件中，对是否记住密码的配置，果然有记住密码的配置
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

### git clone 默认的是master分支，如何切换到branch分支？

git初始化一般是这样。
```
git init

git clone .git地址
```
之后重点来了，因为clone下来的一般为master分支，有可能不是想拉下来的分支。可以使用以下的方法
```
git branch -a 先查看当前远端分支情况

git  checkout origin/xxx  选择远端xxx分支

git branch xxx  创建本地xxx分支

git checkout xxx  选择新创建的分支就可以了。
```
当然还有更简单的方法。
```
直接指定clone某个分支即可：

git clone -b xxx .git地址
```


### tuniu内部的代码npm install报错？

先安装[cnpm](https://www.npmjs.com/package/cnpm)：
 ```
 npm i cnpm -g
 ```
 设置公司内部镜像：
 ```
 cnpm set registry http://10.40.189.154:7001
```
不报错的话，就设置好了，使用命令检查一下：
```
cnpm config get registry //看下是否返回 http://10.40.189.154:7001
```
是的话,再
```
cnpm i
```
安装依赖就行了。



参考：
https://segmentfault.com/q/1010000007962678
https://blog.csdn.net/yun__yang/article/details/74466059
