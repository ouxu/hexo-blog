---
title: 服务器完全配置 （Part 1）
date: 2017-04-21 21:00:05
tags:
  - Nginx
  - Node
categories: 经验分享
comments: false
---

> **目标**：配置一个 LNMP 服务器，支持域名解析，支持二级域名解析，支持 PHP，支持 Node 服务，支持数据库 Mysql...

## 准备

额外声明，本教程很 `low`，大部分都是只说明了怎么用，没有讲原理，还有许多错误解释，请见谅。

<!--more-->

由于本次配置的是云服务器，所以你需要有一个云服务器，在这就不赘述了。

同时还需要一个域名，用来映射服务器资源。

1. 重装服务器，在这里我们选择的系统版本是 `Ubuntu Server 14.04.1 LTS 64位`，其他版本也类似，你喜欢就好，不过不保证下面的安装方法有效。

2. 连接服务器，我用的`ssh`连接。

	```bash
	ssh ubuntu@123.206.55.207
	```
	需要注意的是`ubuntu`默认的用户是 `ubuntu`，如果你是其他的 linux 系统，请把用户名换成 `root` 或相应的用户名。

## 安装
我们使用apt-get来安装各种软件

1. nginx 1.4.6

	```bash
	sudo apt-get install nginx
	```

2. mysql 5.5 (记住你输的 mysql 密码)

	```bash
	sudo apt-get install mysql-server mysql-client
	```

3. php 7.0 (16.04以下的Ubuntu没有7.0的源，需要先自己添加然后安装)

	```bash
	sudo add-apt-repository ppa:ondrej/php
	sudo apt-get update
	sudo apt-get install php7.0-mysql php7.0-curl php7.0-json php7.0-mbstring php7.0-zip php7.0-cgi php7.0 php7.0-fpm
	```
	可能会要等好久，耐心一点吧。

4. git

	```bash
	sudo apt-get install git
	```

## 配置

1. nginx

	先进入 `nginx` 配置文件目录

	```bash
	cd /etc/nginx
	```

	修改 `nginx` 配置，提供两种方法，一种是用变量配置通配服务，另一种是为每一个服务写一个配置文件。现在只介绍通配方法，写配置文件方法在后面，你可以先跳过去看看。

	在`nginx`配置文件(/etc/nginx/sites-enabled/default)中插入如下 `server` 配置，插入即可，不用修改默认 `server`。记得把 `yourdomain.com`替换成你自己的域名。

	```
	server {
	    listen 80;
	    server_name  ~^((?<subdomain>.*)\.)?yourdomain\.com$;
			if ($subdomain = "") {
		        set $subdomain "www";
		   	}
        root   html/$subdomain;
        index  index.html index.htm index.php;
        fastcgi_intercept_errors on;
        error_page  404      = /404.html;
        location / {
                # This is cool because no php is touched for static content.
                # include the "?$args" part so non-default permalinks doesn't
                # break when using query string
                try_files $uri $uri/ =404;
       }

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
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

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        location ~ /\.ht {
            deny  all;
        }
	}
	```

	插入之后，我们重启一下 `nginx` 服务

	```bash
	sudo service nginx restart
	```

	重启成功后，我们需要进入 `nginx` 的资源目录。

	```bash
	cd /usr/share/nginx/html/
	```

	由于我们用的是变量法通配的二级域名，默认的 www 或者不加 www 访问的网址在 `nginx` 资源目录的 `www` 目录下，我们在`www`目录下写一个 index.php 来试试上面的配置是否生效。

	```bash
	sudo mkdir www && cd www
	sudo touch index.php
	```

	index.php 内容

	```php
	<?php
	 phpinfo();
	?>
	```

2. 解析域名

	其实上一步忘了一件事，那就是解析域名，我们需要进行域名云解析。首先添加一条 `@` 记录，直接放图吧。

	![](http://i2.muimg.com/567571/6f1fd107ce5593c7.png)

	然后再加一条 `www`，与上面类似。

	我们为了测试二级域名的配置是否生效，加上自己想要二级域名前缀，我加的是 `wp`，因为待会想要测试一下 WordPress 能否正常搭建。

## 测试
好了，完成了上面的步骤，我们就基本完成了本次服务器的配置，我们浏览我们的域名，查看是否生效。

![](http://i2.muimg.com/567571/8eb425b659b1994c.png)

如果正常的话，应该会打印出 `php` 的配置信息。

到此，我们的 `LNMP` 服务器已经配置好，以后每次想要修改资源文件，只需要在`/usr/share/nginx/html/`中修改对应域名名称的文件夹下的内容即可，比如我要修改 `blog.admin.com` 的内容，只需修改`/usr/share/nginx/html/blog` 中的资源。

对于每一个二级域名，我们都需要在域名管理的云解析中添加对应的解析即可。

---
相关链接：

+ [服务器完全配置 （Part 1） LNMP及二级域名配置](http://www.outxu.cn/nginx-1/)
+ [服务器完全配置 （Part 2） 用Hexo带你了解静态资源怎么映射到域名](http://www.outxu.cn/nginx-2/)
+ [服务器完全配置 （Part 3） Node安装以及在服务器上持久启动 Node 服务](http://www.outxu.cn/nginx-3/)
+ [服务器完全配置 （Part 4） WordPress安装并试试数据库](http://www.outxu.cn/nginx-4/)
+ [服务器完全配置 （Part 5） 处理SPA的二级路由404问题，并教你开启nginx的gzip功能](http://www.outxu.cn/nginx-5/)
