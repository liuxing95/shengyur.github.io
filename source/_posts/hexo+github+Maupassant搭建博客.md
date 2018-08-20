title: Github+Hexo+Maupassant 搭建个人博客教程
date: 2018/4/18
categories: 效率工具
toc: true
tags:
  - Hexo
---

![](http://7xqdjc.com1.z0.glb.clouddn.com/blog_c2a91a3b4a43d03bf967dda7c7425506.png)



<!--more-->
### 安装Node.js
    用来生成静态页面。移步Node.js官网
### 安装hexo
    mac下输入以下命令安装:
```
sudo npm install -g hexo
```
#### *避坑指南*
- 输入管理员密码（Mac登录密码）即开始安装 (sudo:linux系统管理指令 -g:全局安装)
- Hexo官网上的安装命令是$ npm install -g hexo-cli，安装时不要忘记前面加上sudo，否则会因为权限问题报错。


### 初始化
    终端cd到一个你选定的目录，执行hexo init命令：
```
hexo init blog
```
    blog是你建立的文件夹名称。cd到blog文件夹下，执行如下命令，安装npm：
```
npm install
```
    执行如下命令,**开启hexo服务器**
```
hexo s
```
此时，浏览器中打开网址[http://localhost:4000](http://localhost:4000)，能看到如下页面：
![HEXO初始化](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/hexo4000.png)
至此，跟新博客的本地环境就搭好了！

### 关联Github
- 创建仓库
    登录你的Github帐号，新建仓库，名为用户名.github.io固定写法，如我的博客分支名：shengyur.github.io即下图中1所示：
![](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/BLOG.jpg)

    本地的blog文件夹下内容为：

```
_config.yml
db.json
node_modules
package.json
scaffolds
source
themes
```

- 终端cd到blog文件夹下，打开_config.yml，命令如下：

```
open _config.yml
```

- 查看文件最后，修改deploy为：

```
deploy:
  type: git
  repo: https://github.com/shengyur/shengyur.github.io.git
  branch: master
```
- 然后将repo后面的地址中的 shengyur 换成你自己的用户名

#### *避坑指南二*
在配置所有的_config.yml文件时（包括theme中的），在所有的冒号:后边都要加一个空格，否则执行hexo命令会报错。
在blog文件夹目录下执行**生成静态页面命令**：
```
hexo generate     简写：hexo g  
```
此时若出现如下报错：

```
ERROR Local hexo not found in ~/blog
ERROR Try runing: 'npm install hexo --save'
```

则执行命令：

```
npm install hexo --save
```

再执行**配置命令**：
```
hexo deploy       简写：hexo d
```
#### *避坑指南三*
若执行命令hexo deploy仍然报错：无法连接git或找不到git，则执行如下命令来安装[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)：


```
npm install hexo-deployer-git --save      
```

再次执行hexo generate和hexo deploy命令。
若你未关联Github，则执行hexo deploy命令时终端会提示你输入Github的用户名和密码，即

```
Username for 'https://github.com':
Password for 'https://github.com':
```

hexo deploy命令执行成功后，浏览器中打开网址[http://shengyur.github.io](http://shengyur.github.io),
将shengyur换成你的用户名,就能能看到和打开 [http://localhost:4000](http://localhost:4000) 时一样的页面了。

### 添加ssh key到Github
#### 检查SSH keys是否存在Github
执行如下命令，检查SSH keys是否存在。如果有文件id_rsa.pub或id_dsa.pub，则直接进入步骤5.3将SSH key添加到Github中，否则进入下一步生成SSH key。

```
ls -al ~/.ssh
```

#### 生成新的ssh key
执行如下命令生成public/private rsa key pair，注意将your_email@example.com换成你自己注册Github的邮箱地址。

```
ssh-keygen -t rsa -C "your_email@example.com"
```

默认会在相应路径下（~/.ssh/id_rsa.pub）生成id_rsa和id_rsa.pub两个文件。

#### 将ssh key添加到Github中
- Find前往文件夹~/.ssh/id_rsa.pub打开id_rsa.pub文件，里面的信息即为SSH key，将这些信息复制到Github的Add SSH key页面即可。

- 进入Github --> Settings --> SSH keys --> add SSH key:

- Title里任意添一个标题，将复制的内容粘贴到Key里，点击下方Add key绿色按钮即可。

### 发布文章
终端cd到blog文件夹下，执行如下命令**新建文章**：

```
hexo new "postName"
```

名为postName.md的文件会建在目录 /blog/source/\_posts 下。我使用的是vscode来编辑.md文件，语法高亮、错误提示还不错。
文章编辑完成后，终端cd到blog文件夹下，执行如下命令来发布：

```
hexo generate                 //生成静态页面

hexo deploy                   //将文章部署到Github
```

至此，Mac上搭建基于Github的Hexo博客就大功告成了！

### 跟换博客主题
如果不喜欢官网默认主题的话，以到[Hexo官网主题页](https://hexo.io/themes/)去搜寻自己喜欢的theme，或者去知乎看看推荐，找款自己心爱的皮肤，为所欲为~



参考：
- https://blog.csdn.net/yanzi1225627/article/details/54566792
- https://www.jianshu.com/p/13e64c9e2295
