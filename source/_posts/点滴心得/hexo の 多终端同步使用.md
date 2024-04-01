---
title: hexo の 多终端同步使用
index_img: /img/index_img/blog.png
category_bar: 'true'
categories:
  - 点滴心得
date: 2024-03-28 00:00:00
archive:
---



# 方法一

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



# 方法二

在利用Hexo+Github Pages写我们的博客的时候，真正的原始Hexo文件在我们的电脑本地，而GitHub上传的只是Hexo生成的静态网页，即public文件夹里面的内容。

那么假如我们有两台电脑工作，Hexo最开始搭建在其中一台电脑上，而我们需要在另外一台电脑上同时更新我们的博客，该怎么做呢？

也就是说，我们需要实现多台电脑间博客项目的迁移与同步。为实现这一点，我们可以利用Git的分支。



## 配置Git分支

1. 创建分支，并将之设置为默认分支

   博客搭建好后，进入 GitHub 的 yourname.github.io 仓，创建一个名为 hexo 的分支

   将博客项目仓库的 Settings -> Branches -> Default branch 修改为 hexo

2. 将创建的分支的远程仓库克隆到本地

   随便在一个文件夹中 open git bash

   ```git bash
   git clone git@github.com:yourname/yourname.github.io.git
   ```

   文件夹中将生成一个名为 yourname.github.io 文件夹

3. 清空 GitHub 博客仓的 hexo 分支

   1. 进入该 yourname.github.io 文件夹

   2. 勾选查看隐藏的文件

   3. 删去除.git文件夹以外的所有文件

   4. 在克隆的仓库下分别执行以下命令更新删除操作到远程

      ```git bash
      git branch  # 确认你的分支，以防推送失误
      git add -A
      git commit -m "--"
      git push origin hexo
      ```

4. 更新 GitHub 博客仓的 hexo 分支

   1. 将yourname.github.io 文件夹中的 .git 文件夹复制到博客文件夹的根目录

   2. 在博客目录下执行命令同步到远程的hexo分支

      ```git bash
      git add -A
      git commit -m "备份Hexo(提交的描述)"
      git push origin hexo
      ```

5. 查看hexo分支的仓库



## 新电脑的配置

1. **git** **bash**将远程仓库克隆到本地

   ```git bass
   git clone git@github.com:yourname/yourname.github.io.git
   ```

2. 然后进入项目目录，安装依赖启动博客服务器，生成静态文件

   ```git bass
   npm install
   hexo g
   hexo s
   ```

3. 新电脑发布文章

   同之前的教程一样，写好文章后

   ```git bass
   hexo clean
   hexo d -g
   ```



## 两台电脑同步写博客

我们的博客仓库有两个分支，master分支和hexo分支

其中，master分支用于存放Hexo生成的静态资源文件，hexo分支用于存放网站的原始文件

所以，我们在一台设备上写好一篇文章或进行了博客的修改后

执行以下命令，将master中的静态资源文件更新

在博客目录下的cmd中

```git bass
hexo clean
hexo d -g
```

执行以下命令，将hexo中的网站原始文件更新

在博客目录下的git bash中

```git bash
git pull
git add -A
git commit -m "描述"
git push origin hexo
```

## 注意事项

每次有新的操作的时候，别忘了在另一台电脑上更新

```git bash
git pull hexo
```





## 参考

1. [如何解决github+Hexo的博客多终端同步问题](https://blog.csdn.net/Monkey_LZL/article/details/60870891)
2. [hexo博客 多终端同步使用_hexo多终端使用-CSDN博客](https://blog.csdn.net/qq_34291777/article/details/98723401)
2. [多台电脑同步更新Hexo博客_hexo一个分支能实现多端互通吗-CSDN博客](https://blog.csdn.net/qq_30105599/article/details/118302086?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-118302086-blog-60870891.235^v43^pc_blog_bottom_relevance_base5&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-118302086-blog-60870891.235^v43^pc_blog_bottom_relevance_base5&utm_relevant_index=2)
