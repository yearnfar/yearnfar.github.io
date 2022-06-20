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

如果设置的规则不能持久化，重启系统又要重新设置iptables该多烦人，首先要解决的就是iptables的持久化问题。需要按照iptables-persistent，这样重启系统之后，iptables自动生效。

```shell
sudo apt-get install iptables-persistent
```

安装好iptables-persistent发现生成了/etc/iptables目录，目录里面包含rules.v4、rules.v6两个文件。

