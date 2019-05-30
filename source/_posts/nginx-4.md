---
title: 服务器完全配置 （Part 4）
date: 2017-04-21 21:40:10
tags:
  - Nginx
  - Node
categories: 经验分享
comments: false
---

> **目标**：建一个 WordPress 站点试试，带你熟悉 mysql 基本语法。

## 准备

1. WordPress 准备

	下载 `WordPress` 并解压，转移到 `nginx` 目录，更改文件所有者。

	```bash
	cd /usr/share/nginx/html
	sudo wget https://cn.wordpress.org/wordpress-4.7.3-zh_CN.tar.gz
	sudo tar -xzvf wordpress-4.7.3-zh_CN.tar.gz

	sudo mv wordpress wp && rm -rf wordpress-4.7.3-zh_CN.tar.gz
	sudo chown -R www-data:www-data wp
	```

  <!--more-->

2. mysql 准备

	安装 `WordPress` 时需要先创建一个数据库，我们尽量不用 `root` 用户直接操作，所以我们先添加一个 `mysql` 用户，建表并赋予新用户权限。

	连接 `mysql`

	```bash
	mysql -u root -p
	```

	创建用户

	```mysql
	insert into mysql.user(Host,User,Password)
		values("localhost","wp",password("1234"));
	```

	这样就创建了一个名为：wp 密码为：1234 的用户。"localhost"是指该用户只能在本地登录，不能在另外一台机器上远程登录。如果想远程登录的话，将"localhost"改为"%"，表示在任一台电脑上都可以登录。当然也可以指定某台机器可以远程登录。

	创建数据库

	```mysql
	create database wp;
	```

	为用户授权

	```mysql
	grant all privileges on wp.* to wp@localhost identified by '1234';
	flush privileges;  //刷新系统权限表
	```

3. 域名解析

	看 [服务器完全配置 （Part 1） LNMP及二级域名配置](http://www.outxu.cn/nginx-1/)去

## 安装

配好上面的，直接进入 `wp.youadmin.com`，按照说明的填就行了。

哈！又一个 `WordPress` 站点诞生了！

---
相关链接：

+ [服务器完全配置 （Part 1） LNMP及二级域名配置](http://www.outxu.cn/nginx-1/)
+ [服务器完全配置 （Part 2） 用Hexo带你了解静态资源怎么映射到域名](http://www.outxu.cn/nginx-2/)
+ [服务器完全配置 （Part 3） Node安装以及在服务器上持久启动 Node 服务](http://www.outxu.cn/nginx-3/)
+ [服务器完全配置 （Part 4） WordPress安装并试试数据库](http://www.outxu.cn/nginx-4/)
+ [服务器完全配置 （Part 5） 处理SPA的二级路由404问题，并教你开启nginx的gzip功能](http://www.outxu.cn/nginx-5/)
