---
title: "Ubuntu系统iptables入门"
date: 2022-06-20T22:50:45+08:00
lastmod: 2022-06-20T22:50:45+08:00
description: ""
tags: []
featured_image: ""
# images is optional, but needed for showing Twitter Card
images: []
categories:
comment: false
draft: true
author: "yearnfar"
---

一直用云服务商，比如阿里云、AWS等，不需要设置防火墙，只要设置安全组就可以了。

最近够买了一个SBC，放在家里做服务器，需要自己设置防火墙了。

本来最简单的方法是使用UFW，可能是我装的系统ufw和iptables版本不对的原因，开启UFW报错，设置的规则全部无效。只能自己写iptables rule了。

### iptables规则持久化

如果设置的规则不能持久化，重启系统又要重新设置iptables该多烦人，首先要解决的就是iptables的持久化问题。需要安装iptables-persistent，这样重启系统之后，iptables自动生效。

```shell
# 安装 iptables-persistent
sudo apt-get install iptables-persistent

# 开机启动
sudo systemctl enable iptables
```

安装好iptables-persistent发现生成了/etc/iptables目录，目录里面包含rules.v4、rules.v6两个文件。

```shell
# 把ipv4防火墙规则持久化
sudo iptables-save > /etc/iptables/rules.v4

# 把ipv6防护墙规则持久化
sudo ip6tables-save > /etc/iptables/rules.v6
```

### 防火墙策略

防火墙策略有两种，黑名单或者白名单。

黑名单，默认允许全部流量，需要限制的协议、端口需要单独设置。

白名单，默认拒绝全部流量，需要开放的协议、端口需要单独设置。

黑名单相对简单，白名单则更安全。

### 白名单策略

在开启白名单设置前，首先一定要开放SSH端口访问，否则一旦开启防火墙，就无法通过SSH登录服务器了。如果你更改过ssh端口，开放你对应的ssh端口即可。

```shell
# IPv4、IPv6开放22端口
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo ip6tables -A INPUT -p tcp --dport 22 -j ACCEPT
```

开放SSH端口后，就可以设置默认规则了

```shell
# IPv4、IPv6默认将所以的流入数据丢弃
sudo iptables -P INPUT DROP
sudo ip6tables -P INPUT DROP
```

这样，除了SSH端口外，所有的流入数据都被丢弃了，也就是所有除SSH端口外，全部端口都无法访问。

清理iptables规则，如果默认规则是DROP，不要运行这个命令，否则会导致无法使用SSH。

```shell
sudo iptable -F
```

执行了iptables -F 之后，所有的规则都会被清理，如果默认规则是DROP，则会导致无法使用SSH。

所以更好的白名单设置是，默认允许所有流量，再最后加入拒绝所有流量的规则。

```shell
sudo iptables -P INPUT ACCEPT
sudo ip6tables -P INPUT ACCEPT

sudo iptables -A INPUT -j REJECT
sudo ip6tables -A INPUT -j REJECT
```

如果想深入了解iptables相关设定，推荐一个大佬的博客，讲得特别通俗易懂。

### 白名单其他问题

使用黑名单还有个问提，在于有些服务依赖某些端口，但是默认被关闭了，导致服务无法访问。

下面列举几个我遇到的问题：

1、使用了nginx反向代理本地端口，这个时候需要对127.0.0.1，允许所有流入数据。

```shell
sudo iptables -A INPUT -p tcp -s 127.0.0.1 -j ACCEPT
```



[IPtables-朱双印博客](https://www.zsythink.net/archives/category/%e8%bf%90%e7%bb%b4%e7%9b%b8%e5%85%b3/iptables)

