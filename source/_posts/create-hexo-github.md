---
title: Hexo+Github 博客搭建经验分享
date: 2016-08-21 16:45:15
tags:
  - Hexo
  - Github
  - Next
categories: 经验分享
comments: false
---


作为一名程序员，有一个自己的小博客是很正常的，但是如今各种博客让人眼花缭乱，想找到一款自己喜欢的就不容易了。这个博客是参考教程搭建的，总体来说很简单，下面也会放出参考文章的地址．不多说了，进入正题吧。

<!-- more -->

### 1 创建Github 域名和空间

**注册**

如果你已经注册了可以跳过这一步，注意 **username**，这个将会影响你的域名，你的域名将会是 **username.github.io** ，所以认真取一个属于你的专属 ID 吧，注册后验证邮箱就能使用了。

**创建仓库**

注册完之后，我们需要创建一个(repository)来存储我们的网站,点击页面上的 **New repository** 按钮创建仓库，**Respository name** 中的 **username.github.io** 的 username 一定与前面的 Owner 一致，记住你的 username 下面会用到。

![](http://i2.buimg.com/567571/c5ef7d8f1da3b1be.png)

创建完成后，我们就可以进行下一步安装了

### 2 安装 Hexo

首先，我们需要用Hexo初始化一个博客，然后更改一些自定义的配置，或者加上自己喜欢的主题，写上第一篇文章，然后发布到自己的个人Github网站(username.github.io)。

安装 Hexo 前，需要先安装 [node.js](https://nodejs.org/) 和 [git](https://git-scm.com/)，点击文字即可跳转到官方网站。

具体安装方法可以自行搜索，网上都有详细的教程。

如果已经安装完成以上程序，打开bash或者cmd，在命令行中输入

```bash
$ sudo npm install hexo-cli -g
```

安装完毕后,所有必须工具已经准备好了，下面我们就可以生成博客，上传至我们的Github 仓库了。


### 3 创建博客

在本地目录创建一个文件夹，用来存放 Hexo 的相关文件，然后 cd 到你所创建的文件夹，不知道 cd 的可以先去了解 bash 指令。

然后依次执行下列命令
```bash
$ hexo init
```

执行完了之后我们可以启动本地服务查看是否成功。
```bash
$ hexo s
```
![](http://i2.buimg.com/567571/0e7b21cbaf66af3f.png)

打开浏览器，根据给出的地址进入，我这边是http://localhost:4000/ 出现下图就没啥问题。
![](http://i4.buimg.com/567571/701712a8154868b6.png)

### 4 博客美化

好的，显示的网站你可能看着不好看，现在我们更改一下主题。

首先关闭本地 sever 即  **ctrl + c** ，我使用的主题是 [Next](https://github.com/iissnan/hexo-theme-next) 中的 Mist 样式 ，Hexo也有很多[其他](https://hexo.io/themes/)主题供你选择。

执行指令

```bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

**基础配置**

打开项目根目录的 **_config.yml** 修改几个键值对，下面把几个必须设置的列出来按需求修改，记得保存，还有注意配置的键值之间一定要有空格。

```yml
title: 徐的储物间
subtitle: 徐的储物间
description: Everyone in sharing! Everything in sharing!
author: Out_Xu
language: zh-Hans
```

还有文件最后面的

```yml
theme: next

deploy:
  type: git
  repo: https://github.com/ouxu/ouxu.github.io.git
```

**主题配置**

主题配置文件在 **/themes/next/_config.yml** 中修改，这里略过。可以参考[官方文档](http://theme-next.iissnan.com/getting-started.html#theme-settings)

### 5 写文章

所有基础框架都已经创建完成，接下来可以开始写你的第一篇博客了在 **/source/_posts** 下创建你的第一个博客吧,也可以通过命令行生成

```bash
$ hexo new "文章标题"
```

写完了可以先本地测试一下

```bash
$ hexo s
```

测试没问题了，就能发布到 Github 上了，第一次发布需要安装[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)自动部署发布工具

```bash
$ npm install hexo-deployer-git --save
```

安装完就能发布了，博客网址即为 **username.github.io**

```bash
$ hexo d -g
```

以后每次写完文章执行指令提交就行啦！
