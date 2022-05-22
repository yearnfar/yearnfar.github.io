---
title: "Systemctl使用方式"
date: 2022-05-22T17:53:42+08:00
lastmod: 2022-05-22T17:53:42+08:00
description: ""
tags: []
featured_image: "https://cdn.yearnfar.com/blog/22/05/5db8a618b09c8c52153a8d0e479503ad.png"
# images is optional, but needed for showing Twitter Card
images: []
categories:
comment: false
draft:false
author: "yearnfar"
---

以nginx为例，介绍如何使用systemctl 把程序做成服务。

## 配置

创建服务配置文件 nginx.service

```ini
[Unit]
Description=The nginx HTTP and reverse proxy server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
# Nginx will fail to start if /run/nginx.pid already exists but has the wrong
# SELinux context. This might happen when running `nginx -t` from the cmdline.
# https://bugzilla.redhat.com/show_bug.cgi?id=1268621
ExecStartPre=/usr/bin/rm -f /run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/bin/kill -s HUP $MAINPID
KillSignal=SIGQUIT
TimeoutStopSec=5
KillMode=mixed
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```
将nginx.service 放到 /lib/systemd/system/ 目录，systemctl会在这个目录查找配置文件。
```shell
sudo mv nginx.service /lib/systemd/system/
```

## 常用命令

```shell
# 开启自动启动
sudo systemctl enable nginx

# 取消开机启动
sudo systemctl disable nginx

# 启动服务
sudo systemctl start nginx

# 停止服务
sudo systemcl stop nginx

```

