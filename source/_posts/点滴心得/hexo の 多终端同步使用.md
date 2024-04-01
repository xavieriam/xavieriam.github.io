---
title: hexo の 多终端同步使用
index_img: /img/index_img/blog.png
category_bar: 'true'
categories:
  - 点滴心得
date: 2024-03-28 00:00:00
archive:
---



## 配置思路

远程分支：maser、hexo存放不同文件内容。

- hexo分支：存放本地文件，包括建站所需的配置文件、资源文件夹、主题文件夹、模板文件夹等，具体如下：

  {% label info @ 【注】这里没有public文件夹，其实本地执行hexo d -g等发布命令会将由md文件渲染的静态文件放入public中。 %}

  ```md
  .
  ├── _config.yml  网站的 配置 信息，您可以在此配置大部分的参数
  ├── package.json 应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装
  ├── scaffolds    模版 文件夹
  ├── source       资源文件夹是存放用户资源的地方
  |   ├── _drafts
  |   └── _posts
  └── themes       主题 文件夹
  ```

  这样hexo分支具有备份本地文件作用，当用新的设备时，只要与hexo分支同步既可以继续接着以前设备上的文章继续写作。

- master分支：存放hexo服务器上发布的静态文件等，也就是在本地目录下的public目录。这里master起服务器目录的作用。



## 具体步骤

1. 旧的设备做好备份本地文件到远程的hexo分支下，假定旧的设备是自己以及搭建hexo博客成功到master上了。做完Github Pages页面了。所以master分支下已有hexo生产的静态文件等。

   ```nginx
   git init  //初始化本地仓库
   git add source //将必要的文件依次添加，有些文件夹如npm install产生的node_modules由于路径过长不好处理，所以这里没有用`git add .`命令了，而是依次添加必要文件，如下图所示.使用git add -A 可以自动把node_modules文件夹忽略。选择依次add或add -A都可以，下面图是add -A提交后的，测试是可以的
   git commit -m "Blog Source Hexo"
   git branch hexo  //新建hexo分支
   git checkout hexo  //切换到hexo分支上
   git remote add origin git@github.com:yourname/yourname.github.io.git  //将本地与Github项目对接
   git push origin hexo  //push到Github项目的hexo分支上
   ```

2. 另一台新设备发布前准备

   1. 安装node.js https://nodejs.org/en/download/
   2. 安装hexo npm install hexo -g
   3. 安装git
   4. clone hexo分支，安装必要组件，写文章，add并commit，本地文件备份到hexo，发布到maser

   ```nginx
   git clone -b hexo git@github.com:yourname/yourname.github.io.git  //将Github中hexo分支clone到本地
   cd  yourname.github.io  //切换到刚刚clone的文件夹内
   npm install    //注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
   hexo new post "new blog name"   //新建一个.md文件，并编辑完成自己的博客内容
    git add source  //经测试每次只要更新sorcerer中的文件到Github中即可，因为只是新建了一篇新博客
   git commit -m "XX"
   git push origin hexo  //更新分支
   hexo d -g   //push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master
   ```

3. 这样就可以在不同终端下，同步的写文章了

   ```nginx
   git pull origin hexo
   hexo new post "new post name"
   git add source
   git commit -m "XXXX"
   hexo d -g
   git push origin hexo
   ```


   

二、部署过程中出现的问题

【问题 1】每次hexo d -g 生产静态文件并部署推送到远程master分支下后，Github Pages设置的域名失效，每次推送到master都要自己setting中设置或添加CNAME文件。

- 解决方案：为了避免域名失效后自己手动的设置，可以将CNAME文件放到本地文件的source目录下，hexo部署时会一并把CNAME文件部署到master分支的根目录下。如图CNAME在我本地文件夹的位置及CNMAE文件内容。README文件同理也是这样。



【问题 2】当本地提交了不想提交的\错误的到远程分支下，想让远程分支回退到某个版本

- 解决链接

  https://blog.csdn.net/ccorg/article/details/80115408

  https://blog.csdn.net/fuchaosz/article/details/52170105



## 参考

1. [如何解决github+Hexo的博客多终端同步问题](https://blog.csdn.net/Monkey_LZL/article/details/60870891)

