title: Hexo的版本控制与持续集成
date: 2018/6/17
categories: 效率工具
toc: true
tags:
  - Hexo
---

上一篇文章 [利用hexo在多台电脑上提交和更新github pages博客](https://shengyur.github.io/2018/06/16/hexo%E5%8D%9A%E5%AE%A2%E7%9A%84%E9%83%A8%E7%BD%B2%E4%BC%98%E5%8C%96%E4%B8%8E%E7%AE%A1%E7%90%86/)中，我们学习了如何利用hexo在多台电脑上提交和更新博客内容，那么每次在本地更新完博客后，需要执行两部操作，提交好源文件后，再hexo d进行发布，是不是很麻烦？？换了电脑之后又要重新搭建环境，是不是很蛋疼？？

那么接下来我们就来说说如何优雅愉快地对我们的Hexo进行版本管理和发布。
<!--more-->

既然我们已经用了GitHub来托管我们生成出来的静态网站，那么为什么不也把Hexo博客的源文件也host在GitHub上呢。那么问题来了，如果我们把Hexo博客的源文件托管在GitHub上，我们的发布流程就会变为：

1. 执行git push把更新的源文件push到托管源文件的GitHub Repo (我们称之为Source Repo)
2. 执行hexo deploy来更新托管静态网站的GitHub Pages (我们称之为Content Repo)

这样看来，每次更新博客要经历两个步骤，并不是那么straightforward。**那么有没有办法做到既能使用GitHub进行版本控制，又能做到一键发布呢？**

答案是肯定的。这里用到了 [**持续集成**](https://en.wikipedia.org/wiki/Continuous_integration) 也就是我们一直所说的CI来完成一键发布：
当有新的change push到Source Repo时，自动执行CI脚本，生成最新的静态网站发布到Content Repo，一气呵成。

那么我使用什么CI工具来做呢？我们可以使用像Travis CI这样的Hosted CI Service，也可以使用Jenkins或者TeamCity来搭建CI server。
如果自己来搭建CI Server，费时费力，又要花钱来买Server来host CI service，肯定不是一个很好的选择。那么我们选哪个Hosted CI Service呢？

经过对比，发现使用[AppVeyor](https://www.appveyor.com/)来建立CI非常方便，主要是以下步骤：

1. 注册并登陆AppVeyor
访问[AppVeyor](https://formulahendry.github.io/assets/img/hexo-ci/appveyor-login.png)登陆页面，使用你的GitHub账号登陆即可。

2. 添加Project

3. 添加appveyor.yml到Source Repo




参考：
- [阮一峰 Travis CI教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)
- [ Hexo的版本控制与持续集成 ](https://formulahendry.github.io/2016/12/04/hexo-ci/)
