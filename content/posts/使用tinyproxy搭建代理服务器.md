---
title: "使用tinyproxy搭建代理服务器"
date: 2022-05-29T00:26:10+08:00
lastmod: 2022-05-29T00:26:10+08:00
description: "大家或多或少都会遇到需要科学上网的情况，使用tinyproxy搭建代理服务器，可以解决科学上网问题"
tags: ["tinyproxy", "代理服务器", "科学上网"]
featured_image: "https://cdn.yearnfar.com/blog/22/05/a8bc85d20a8bd482b9f2f973378f170b.jpg"
# images is optional, but needed for showing Twitter Card
images: []
categories:
comment: false
draft: false
author: "yearnfar"
---

程序员都有遇到，需要在终端设置http_proxy的情况。这里介绍使用tinyproxy使得在终端科学上网。

### 源码编译安装Tinyproxy

[Tinyproxy项目地址](https://github.com/tinyproxy/tinyproxy)

下载安装包到本地

```shell
[yearnfar@aws ~]$ wget https://github.com/tinyproxy/tinyproxy/releases/download/1.11.1/tinyproxy-1.11.1.tar.gz
[yearnfar@aws ~]$ tar zxvf tinyproxy-1.11.1.tar.gz
[yearnfar@aws ~]$ cd tinyproxy-1.11.1
[yearnfar@aws ~]$ ./configure --prefix=/usr/local/tinyproxy
[yearnfar@aws ~]$ make
# 如果提示性没有 gcc
# [yearnfar@aws ~]$ make yum install gcc
[yearnfar@aws ~]$ sudo make install
[yearnfar@aws ~]$ cd /usr/local/tinyproxy
[yearnfar@aws tinyproxy]$ ls
bin  etc  share
[yearnfar@aws tinyproxy]$ sudo cp -r etc/tinyproxy /etc/   # 将配置文件移动到/etc目录
```

修改配置文件

```ini
User nobody
Group nobody

# 为了避免被扫描到，最好修改一个端口
Port 8888   

Timeout 600

DefaultErrorFile "/usr/local/tinyproxy/share/tinyproxy/default.html"

StatFile "/usr/local/tinyproxy/share/tinyproxy/stats.html"

LogFile "/var/log/tinyproxy.log"

LogLevel Info

PidFile "/var/run/tinyproxy.pid"

MaxClients 100

# 需要设置一个密码，防止被扫描到，被搭便车
BasicAuth youname your_password

# 必须设置，否则默认禁止全部代理
FilterDefaultDeny No

# 需要注释掉，否则只能本机使用
#Allow 127.0.0.1
#Allow ::1
```

启动代理服务器

```shell
[yearnfar@aws tinyproxy]$ sudo /usr/local/tinyproxy/bin/tinyproxy -c /etc/tinyproxy/tinyproxy
```

### 测试代理

```shell
➜  ~ curl cip.cc
IP	: 218.19.46.x
地址	: 中国  广东  广州
运营商	: 电信

数据二	: 广东省广州市 | 电信

数据三	:

URL	: http://www.cip.cc/218.19.46.x

➜  ~ export http_proxy="http://yourname:your_password@you_ip:8888"
➜  ~ curl cip.cc
IP	: 13.214.254.x
地址	: 美国  美国

数据二	: 美国 | Xerox

数据三	: 美国康涅狄格 | 亚马逊

URL	: http://www.cip.cc/13.214.254.x
```

可以看到，代理正常。如果不正常，可以按以下顺序检查：

- Tinyproxy服务是否启动
- 云服务器安全组设置是否开放Tinyproxy的监听端口（例如:8888）。

### 设置开机自启

创建文件tinyproxy.service

```ini
[Unit]
Description=Tinyproxy daemon
Requires=network.target
After=network.target

[Service]
Type=forking
PIDFile=/var/run/tinyproxy.pid
ExecStart=/usr/sbin/tinyproxy -c /etc/tinyproxy/tinyproxy.conf
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```shell
[yearnfar@aws ~]$ sudo cp tinyproxy.service  /lib/systemd/system/tinyproxy.service
[yearnfar@aws ~]$ sudo systemctl enable tinyproxy
[yearnfar@aws ~]$ sudo systemctl start tinyproxy
```



