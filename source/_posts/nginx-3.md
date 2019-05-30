---
title: 服务器完全配置 （Part 3）
date: 2017-04-21 21:38:10
tags:
  - Nginx
  - Node
categories: 经验分享
comments: false
---

> **目标**：带你安装 `Node` 以及处理 `Node` 服务的端口反向代理。

## 前言
`Node` 越来越火了，所以给服务器配置 `Node` 也很有必要了，一般的 `Node` 服务都是启动在本地服务的某一端口，本例将以一个简单的 `Express` 服务带你部署持久启动` Node` 服务，并将其反向代理到相应域名上。

<!--more-->

## 安装Node
我们直接采用 `npm` 的一个神奇的包——`n`，来安装 `node`。

1. 从 `github` 克隆 `n` 的仓库并安装，注意最好在 `/usr/local` 目录下操作。

	```bash
	cd /usr/local
	sudo git clone https://github.com/tj/n.git n
	cd n
	sudo make install
	```

	上面执行最后一步可能会提示没有 `make`，那就装一个呗。

	```bash
	sudo apt-get install make
	sudo make install
	```

2. 用 `n` 指令安装最新版 `node` 及 `npm`。

	```bash
	sudo n latest
	```

好了，这样 `Node` 即安装完成了，我们可以试着输出 `Node` 版本号。

```bash
node -v
```

注意，用这种方法安装的话，每次安装全局包时需要加上 `sudo`。我们来安装 `nrm` 来试试吧，`nrm` 是 `npm` 的源管理工具，我们用 `nrm` 切换到 `npm` 淘宝源。

```bash
sudo npm install nrm -g --registry  https://registry.npm.taobao.org
nrm use taobao
```
没有问题的话应该是能够输出切换成功的信息。

## 部署我们的 Express 服务（一个 Node 后端框架）

我们就从零开始部署吧，通过应用生成器工具 `express` 可以快速创建一个应用的骨架。

```bash
sudo npm install express-generator -g
```
进入你想把 `express` 放置的目录，我就放在了 `/usr/share/nginx/html`，这个放置地方对 `express` 启动的服务没有影响，因为 `node`启动在本地服务的某一端口。

用`express` 生成器生成一个 `express` 项目，并启动它。

```bash
sudo express api
cd api && sudo npm install
DEBUG=api:* npm start
```

服务已经启动在 `3000` 端口，我们访问试试，在浏览器地址输入`123.206.55.207:3000`。如果没问题你应该能看见如下页面。

![](http://i1.piimg.com/567571/d7cd6fbf9cab67e1.png)

## 反向代理+持久部署

直接通过上面的方式启动并不能持久化开启服务，所以我们用 `pm2` 来托管我们的服务。首先通过 `npm` 安装 `pm2`。

如果你没停止刚刚开启的 `express`服务，按 `Ctrl+C`停止。

```bash
sudo npm install pm2 -g
```

通过 `pm2` 启动服务。

```bash
pm2 start bin/www
```

好了，我们已经把服务持久部署了，接下来我们把服务映射到域名上去。

我以 `api.outxu.cn` 作为该服务的请求地址，先去域名管理中添加解析记录 `api`。

然后进入 `nginx` 配置目录，添加一个虚拟主机。

```bash
cd /etc/nginx/conf.d
sudo nano api.conf
```

写入以下内容：

```
server {
        listen 80;
        server_name  api.youradmin.com;
        location / {
                proxy_pass http://0.0.0.0:3000/;
        }
}
```

重启 `nginx` 服务：

```bash
sudo service nginx restart
```

我们访问试试，在浏览器地址输入`api.youradmmin`。如果没问题你应该能看见与上面一样的界面。

至此，`Node` 安装以及在服务器上持久启动 `Node` 服务讲解结束

---
相关链接：

+ [服务器完全配置 （Part 1） LNMP及二级域名配置](http://www.outxu.cn/nginx-1/)
+ [服务器完全配置 （Part 2） 用Hexo带你了解静态资源怎么映射到域名](http://www.outxu.cn/nginx-2/)
+ [服务器完全配置 （Part 3） Node安装以及在服务器上持久启动 Node 服务](http://www.outxu.cn/nginx-3/)
+ [服务器完全配置 （Part 4） WordPress安装并试试数据库](http://www.outxu.cn/nginx-4/)
+ [服务器完全配置 （Part 5） 处理SPA的二级路由404问题，并教你开启nginx的gzip功能](http://www.outxu.cn/nginx-5/)
