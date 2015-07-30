---
layout: post
title: 在CentOS服务器中创建Git仓库并同步网站
categories: [server]
tags: [CentOS, Git]
---

本篇博客记录在CentOS服务器中部署Git仓库，并同步apache中的网站目录的过程

###搭建Git服务器
----------------
1. 安装git

2. 创建一个git用户:
> sudo adduser git

3. 创建证书登陆:
收集所有需要登录的用户的公钥（id_rsa.pub），把所有公钥导入到~/.ssh/authorized_keys文件中，一行一个

4. 初始化git仓库:
> sudo git init --bare test.git

> sudo chown -R git:git test.git

5. 禁用git用户的Shell登陆:
修改/etc/passwd中的
> git:x:1001:1001:,,,:/home/git:/bin/bash

改为：

> git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell

6. 构建Hook:
> cd test.git

> cat > hooks/post-receive

\#!/bin/bash

GIT_WORK_TREE=/var/www/blog git checkout -f

> chmod +x hooks/post-receive

###本地clone并推送
------------------
1. cd work

2. git clone git@server_address:/root/test/test.git

3. cd test

4. touch README.md

5. git add .

6. git commit -m "initial commit"

7. git push origin master

###二级域名
-----------------
1. [万网设置](http://jingyan.baidu.com/article/b24f6c82db5ecc86bfe5dae7.html)

2. Apache设置
{% highlight xml %}
NamevirtualHost *:80

<VirtualHost *:80>
	ServerName a.com
	DocumentRoot /var/www/site
</VirtualHost>

<VirtualHost *:80>
	ServerName bbs.a.com
	DocumentRoot /var/www/bbs
</VirtualHost>
{% endhighlight %}





