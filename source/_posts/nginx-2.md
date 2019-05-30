---
title: 服务器完全配置 （Part 2）
date: 2017-04-21 21:07:28
tags:
  - Nginx
  - Node
categories: 经验分享
comments: false
---

> **目标**：带你了解静态资源怎么映射到域名，利用 git 很简单！顺便教你普通的二级域名配置方法。

## 前言
服务器部署后最简单的功能就是映射静态资源到域名，而 `Hexo` 本身就是直接把你写的 `Markdown` 处理成静态文件，所以这里就用 `Hexo` 做演示。[点此了解 Hexo](https://hexo.io/)

进行到这一步我假设你已经搭建好了 `Hexo` 博客系统并已经部署到了 `github page` 上，我们进入对应的 `github` 仓库。

<!-- more -->

![](http://i4.buimg.com/567571/f4b5cb3d80a6b36b.png)

显而易见，所有的静态文件都在这里，而且不需要做任何处理就能部署到服务器。

## 部署
首先当然是连接主机了，还是用 `ssh` 连接。

```bash
ssh ubuntu@123.206.55.207
```
进入对应的目录，我直接放在 `blog` 目录下，映射的是 `blog.admin.com`。

```bash
cd /usr/share/nginx/html/
```

将静态文件 `clone` 并重命名项目为 `blog`，记得加上 `sudo`执行。

```bash
sudo git clone https://github.com/ouxu/ouxu.github.io.git blog
```

如果用的是通配法配置的二级域名，这时候访问 `blog.admin.com` 既能看到已经部署成功。

以后每次写了新的东西，并更新到了 github 上，我们只需要在 `blog` 目录下执行 `git pull` 指令就能同步文件。

除了利用 `git` 进行文件部署外，我们还能用 `scp` 指令进行操作，下面我们动手试试吧。

假如我要把我本地的 `hexo` 静态文件传输到服务器上，我们需要先进入本地` hexo `的目录下。

然后执行 `scp` 指令。需要注意的是服务器对应的路径需要开放权限，我们把目标目录权限改成 `777`。

在服务器中修改：

```bash
sudo chmod -R 777 /usr/share/nginx/html/temp
```
然后在本地执行：

```bash
scp -r ./public ubuntu@123.206.55.207:/usr/share/nginx/html/temp
```

好了然后把文件从 `temp` 中复制或移动到 `blog` 目录下：

```bash
sudo cp -r /usr/share/nginx/html/temp  /usr/share/nginx/html/blog
```

这样也能把文件传输到服务器上。

## 附加
> 配置文件说明
>
> 首先是主配置文件 文件位置/etc/nginx/nginx.conf
>
> 此文件中是nginx的核心配置项（比如最大连接数等等），其实整个nginx的配置项都是通过这个文件确定的，其中的虚拟主机部分（即server）是在该文件中通过include实现的。
>
> 两个虚拟主机的配置目录，一个是sites-available 一个是sites-enabled ，两者内部都默认有一个default文件，是默认的虚拟主机配置，注意这两个文件其实就是一个文件，enabled中的default只是一个链接而已。所以在nginx.conf中，只是include了enabled目录里的所有文件，按照这个默认的逻辑，如果想要增加一个虚拟主机的配置，那么你应该单独写一个配置文件放在enabled目录下即可，或者可以写到 `conf.d` 目录，这些全看个人习惯吧。


明白了这些，我们进入 nginx 配置目录。

```bash
cd /etc/nginx/conf.d
```
新建服务的 `conf` 文件。

```bash
sudo touch blog.conf
```
写入自定义的 server 信息，我这直接写的配置好了 `php` 的，你也可以自己修改。

```bash
server {
        listen 80;
        server_name blog.mycollagelife.com;
        root /usr/share/nginx/html/blog;
        index index.html index.htm index.nginx-debian.html index.php;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }
        location ~ \.php$ {
                           fastcgi_split_path_info ^(.+\.php)(/.+)$;
                # NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
                # With php5-cgi alone:
        #       fastcgi_pass 127.0.0.1:9000;
        #       # With php5-fpm:
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
        }
}

```

然后重启 nginx 服务，就能见到效果啦~

---
相关链接：

+ [服务器完全配置 （Part 1） LNMP及二级域名配置](http://www.outxu.cn/nginx-1/)
+ [服务器完全配置 （Part 2） 用Hexo带你了解静态资源怎么映射到域名](http://www.outxu.cn/nginx-2/)
+ [服务器完全配置 （Part 3） Node安装以及在服务器上持久启动 Node 服务](http://www.outxu.cn/nginx-3/)
+ [服务器完全配置 （Part 4） WordPress安装并试试数据库](http://www.outxu.cn/nginx-4/)
+ [服务器完全配置 （Part 5） 处理SPA的二级路由404问题，并教你开启nginx的gzip功能](http://www.outxu.cn/nginx-5/)
