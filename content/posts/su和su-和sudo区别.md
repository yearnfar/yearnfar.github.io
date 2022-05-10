---
title: "Su和su 和sudo区别"
date: 2022-05-10T19:24:06+08:00
lastmod: 2022-05-10T19:24:06+08:00
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

最近强迫症又犯了，把自己跑了4年的服务器重装了下系统。重装之后，开始认真的研究如何使用。

以前很少使用sudo这个命令，如果要root权限一般都是直接su 输入root密码。但是sudo明显要更加方便。于是就开启了sudo权限。

### 开启sudo

visudo 在root下添加一行

```shell
## Allow root to run any commands anywhere
root	ALL=(ALL) 	ALL
your_name ALL=(ALL) NOPASSWD: ALL
```

#### 修改sudo PATH

我在/usr/local/bin目录安装了docker-compose 

但是使用sudo docker-compose 却提示命令不存在，这时还是要使用visudo 修改secure_path，增加/usr/local/bin，这样/usr/local/bin就加到sudo 的PATH里面了。

```shell
Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin:/usr/local/bin
```





 



