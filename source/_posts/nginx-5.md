---
title: 服务器完全配置 （Part 5）
date: 2017-04-21 21:43:26
tags:
  - Nginx
  - Node
categories: 经验分享
comments: false
---

> **目标**：处理 `SPA` 的二级路由 `404` 问题，并教你开启 `nginx` 的 `gzip` 功能。

## 前言

现在前端 `SPA` 应用越来越多，各家框架都有自己的一套路由体系，像 `React` 就由 `Router-router` 进行路由管理，这样大大方便了我们的开发，但是也带来了问题。

一般我们会遇到像下面这样的路由：`http://xx.xxx.com/xxxx/xxxx`，如果没有做任何设置，直接访问这个 `url` 会返回 `404` 页面，因为 `nginx` 启动的服务其实是在你的资源文件夹中寻找对应的文件，而 `SPA` 打包后只有一个或几个 `.js` 文件，nginx 是找不到的，所以理所当然给你返回 `404` 页面。

<!-- more -->
## 解决

其实很简单，我们只需把 `nginx` 配置项中的 `try_files` 定向至 `index.html`，具体如下：
```
server {
        listen 80;
        root /usr/share/nginx/html/oj;
        index index.html index.htm index.nginx-debian.html;
        server_name oj.outxu.cn;
        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri /index.html;
                # Uncomment to enable naxsi on this location
                # include /etc/nginx/naxsi.rules
        }

}
```



## Gzip

好了，这部分有点水，所以附带 `nginx` 开启 `gzip` 的教程，也是很水...

`nginx` 自带了 `Gzip` 功能，我们只需要修改 `nginx` 默认配置就行了，直接编辑 `nginx.conf` 文件。

```bash
nano /etc/nginx/nginx.conf
```

找到 `gzip` 设置项，改成下面这样：

![](http://i2.muimg.com/567571/ec0454a1030feb95.png)

重启 `nginx` 服务：
```bash
sudo service nginx restart
```
---
相关链接：

+ [服务器完全配置 （Part 1） LNMP及二级域名配置](http://www.outxu.cn/nginx-1/)
+ [服务器完全配置 （Part 2） 用Hexo带你了解静态资源怎么映射到域名](http://www.outxu.cn/nginx-2/)
+ [服务器完全配置 （Part 3） Node安装以及在服务器上持久启动 Node 服务](http://www.outxu.cn/nginx-3/)
+ [服务器完全配置 （Part 4） WordPress安装并试试数据库](http://www.outxu.cn/nginx-4/)
+ [服务器完全配置 （Part 5） 处理SPA的二级路由404问题，并教你开启nginx的gzip功能](http://www.outxu.cn/nginx-5/)
