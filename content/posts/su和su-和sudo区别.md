---
title: "su和su -和sudo区别"
date: 2022-05-10T19:24:06+08:00
lastmod: 2022-05-10T19:24:06+08:00
description: "linux环境su和su -和sudo区别"
tags: ["linux", "sudo", "su"]
featured_image: ""
# images is optional, but needed for showing Twitter Card
images: []
categories:
comment: false
draft: false
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

### su 与 su - 与 sudo

su 不加任何参数，切换到root，不变环境变量(与切换前一致)。

su - ，切换到root，并且切换到root的环境变量。

sudo命令使用时将PATH环境变量进行了重置，我们使用sudo visudo 在secure_path可以看到sudo所设置的PATH环境变量。

```shell
Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin:
```







 



