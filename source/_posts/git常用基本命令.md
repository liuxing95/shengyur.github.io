title: git常用基本命令
date: 2018/4/17
categories: 效率工具
tags:
  - git
---

### 必须记住的六条命令
1. cd:用来切换工作目录,最常用的一个命令。简单来讲，cd A文件夹就是进入到A文件夹里面的意思。
2. git status .：查看当前路径下的的状态。git下最最常用的一个命令。
3. git add .: 把工作区的所有变化，(就是你的所有改动)，都添加到 版本库/暂存区。
4. git commit -m "提交时说明信息": 更进一步提交，并说明提交log。
5. git push: 把版本库的所有更新内容， 都推送到远程服务器。(就是推代码/推上去)
6. git pull: 把代码从远程服务器拉取到本地。(俗称拉代码)
<!--more-->
### 当我们在本地新建了自己的代码仓库，需要传到github上时，我们的操作：
1. 建立git仓库
cd到你的本地项目根目录下，执行git命令，此命令会在当前目录下创建一个.git文件夹。
git init

2. 将项目的所有文件添加到仓库中
git add .
这个命令会把当前路径下的所有文件，添加到待上传的文件列表中。 如果想添加某个特定的文件，只需把.换成特定的文件名即可

3. 将add的文件commit到仓库
git commit -m "注释语句"

4. 去github上创建自己的Repository，点击NewRepository。
点击Create repository，拿到创建的仓库的https地址

5. 将本地的仓库关联到github上
git remote add origin https://自己的仓库url地址

6. 上传代码到github远程仓库
git push -u origin master

7. 执行完后，如果没有异常，等待执行完就上传成功了，中间可能会让你输入Username和Password，你只要输入github的账号和密码就行了.

### git如何全局设置用户名及邮箱：
git config user.name "Your Name"
git config user.email you@example.com

### 当我们修改了本地代码，向远程服务器推送时，我们的操作步骤如下:
1. git add .（这个命令会把当前路径下的所有文件，添加到待上传的文件列表中。 如果想添加某个特定的文件，只需把.换成特定的文件名即可）
2. git commit -m "提交时说明信息"
3. git push
4. 当我们想更新本地代码，就是把服务器上最新的代码拉取下来，只需要执行一个命令: git pull

### 这三条命令建议记住
1. git log:查看提交历史，与各次的提交说明。
2. git diff:比较工作区与暂存区的差异，就是比较看看你到底都做了什么修改。
3. git clone url地址: 将远程服务器上项目克隆到新创建的目录中（第一次拉项目时使用， 后面的更新都用 git pull了）。

### 其他问题
- 操作时 双击tab键的自动提示/补全功能。
- q或者:q等命令代表退出(quit)。
- ctrl+f,ctrl+b快捷键在termial可以翻页，就是 上一页，下一页

# 正文部分

## 理解几个概念
工作区（Working Directory）， 版本库（Repository）/暂存区 ，（中央/远程）服务器.

*服务器*:概念已经清楚了。叫做 中央服务器/远程服务器都行。
*工作区*:就是你电脑的工作目录
*版本库*:工作区有一个隐藏的 .git文件夹，这个是叫做 版本库(有些文章也叫 暂存区，不管叫什么，知道这个意思就好)。.git 是隐藏文件夹。该文件内的内容很重要，因为git的控制配置等信息，都在这个隐藏文件夹里。电脑如果设置不显示隐藏文件夹，那么就会看不到。

## 为什么存在一个 版本库？
我修改过的代码，直接从 工作区提交到服务器不就行了嘛，为什么还要这么麻烦。svn 等集中式版本管理系统就是这么做的，简单明了，但是如果你没网络时怎么办？所以有了 版本库，那么你可以把代码先从工作区提交到版本库，等待有网络了，可以再提交到服务器。

## .gitignore文件是干啥的?
工作区的目录下面，总会存在很多乱七八糟的文件，比如你本地的配置，编译生成的中间文件等，这些文件你不想(或不能)提交到 服务器。那怎么办呢。就把这些文件的规则写到 .gitignore文件中，这样git就会 ignore(忽略)这些文件，git就会像没看到这些文件一样。

## .gitignore文件的使用
```
1）/mtk/               过滤整个文件夹
2）*.zip                过滤所有.zip文件
3）/mtk/do.c         过滤某个具体文件
```
被过滤掉的文件就不会出现在git仓库中（gitlab或github）了，当然本地库中还有，只是push的时候不会上传。
需要注意的是，gitignore还可以指定要将哪些文件添加到版本管理中：
```
1）!*.zip
2）!/mtk/one.txt
```
唯一的区别就是规则开头多了一个感叹号，Git会将满足这类规则的文件添加到版本管理中。
为什么要有两种规则呢？想象一个场景： **假如我们只需要管理/mtk/目录中的one.txt文件，这个目录中的其他文件都不需要管理** ，那么我们就需要使用：
```
1）/mtk/
2）!/mtk/one.txt
```
假设我们只有过滤规则，而没有添加规则，那么我们就需要把/mtk/目录下除了one.txt以外的所有文件都写出来！

配置语法：
以斜杠“/”开头表示目录；
以星号通配多个字符；
以问号“?”通配单个字符
以方括号“[]”包含单个字符的匹配列表；
以叹号“!”表示不忽略(跟踪)匹配到的文件或目录；

### 待补充
git config 文件配置项

- 参考：
- 小白教程：https://www.cnblogs.com/yaoxiaowen/p/8227873.html
- 秒秒钟入门markdown语法：https://www.jianshu.com/p/q81RER
- Git忽略规则.gitignore梳理:https://www.cnblogs.com/kevingrace/p/5690241.html
- 王老师的博客 https://github.com/wang-qingqing/accumulate/blob/master/%E5%B7%A5%E5%85%B7%E7%B1%BB/Git/git%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95.md
- git config https://blog.csdn.net/gaochenchen/article/details/76187480
