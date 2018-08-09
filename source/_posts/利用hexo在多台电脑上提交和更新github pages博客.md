title: 利用hexo在多台电脑上提交和更新github pages博客
date: 2018/6/16
categories: 效率工具
toc: true
tags:
  - Hexo
---


#### 概述
Hexo部署到GitHub上的文件，是本地的.md文件编译生成的html文件（静态网页）。因此，当你重装电脑或者想在不同电脑上修改博客时，没有编译之前的母本文件，操作就很麻烦了。

其实，Hexo生成的网站文件中有.gitignore文件，因此它的本意也是想我们将Hexo生成的网站文件存放到GitHub上进行管理的（而不是用U盘或者云备份）。这样，不仅解决了上述的问题，还可以通过git的版本控制追踪你的博文的修改过程，操作简单，效率更高。

但是，如果每一个GitHub Pages都需要创建一个额外的仓库来存放Hexo网站文件，就会多出许多不必要的仓库（10个项目需要20个仓库）。所以，本文选择利用分支。

简单地说，每个想建立GitHub Pages的仓库，起码有两个分支，一个用来存放Hexo网站的文件，一个用来发布网站。

下面以我的博客作为例子详细地讲述。


#### 搭建流程

1 . 创建仓库，shengyur.github.io；

2 . 创建两个分支：master 与 sourceCode;

3 . 设置sourceCode为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；

4 . 使用git clone xxx.github.io.git拷贝仓库；

5 . 在本地xxx.github.io文件夹下通过Git bash依次执行npm install hexo、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;

6 . 修改_config.yml中的deploy参数，分支应为master；

7 . 依次执行git add .、git commit -m “…”、git push origin sourceCode提交网站相关的文件；
如果出现报错
```
! [rejected]          sourceCode -> sourceCode (non-fast-forward)
```
解决办法：

1、用Merge合并

      命令行下，可以使用以下命令：

      git fetch

      git merge

2、强推

      git push -f

      即利用强覆盖方式用你本地的代码替代git仓库内的内容。

8 . 执行hexo generate -d生成网站并部署到GitHub上。

这样一来，在GitHub上的xxx.github.io仓库就有两个分支，一个sourceCode分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页。多端登陆修改博客就可以实现了~


参考：
1. https://www.jianshu.com/p/0b1fccce74e0
2. https://formulahendry.github.io/2016/12/04/hexo-ci/
3. http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more
