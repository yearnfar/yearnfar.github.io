---
title: "中国移动宽带设置ipv6"
date: 2022-06-03T22:39:34+08:00
lastmod: 2022-06-03T22:39:34+08:00
description: "中国移动家庭宽带设置ipv6地址"
tags: ["IPv6","中国移动"]
featured_image: "https://cdn.yearnfar.com/blog/22/06/a3fb23f9715c731355f6965ddd2e8411.webp"
# images is optional, but needed for showing Twitter Card
images: []
categories:
comment: false
draft: false
author: "yearnfar"
---

由于国家大力推进IPv6，目前我们使用手机流量上网时都可以分配到IPv6地址。大家可以通过手机自带浏览器访问 https://test-ipv6.com/ 查看手机是否支持IPv6。

除了手机流量，我们还会使用WiFi上网，WiFi上网时，就未必是有IPv6地址了。目前主流的网络供应商应该也是支持IPv6的，问题大概率是出现在用户自己的路由是否支持IPv6和以及路由的设置上。

这以我的情况讲解下如何开启IPv6。

我使用的是中国移动宽带，接入户的是光纤。所以有一个光猫 + 自己购买的路由器。要支持IPv6需要满足3个条件：

- 移动入户的网络是否支持IPv6。
- 光猫支持IPv6。
- 自己购买的路由支持IPv6。

一般默认情况下，我们都是使用光猫拨号上网，路由自动获取IP地址的方式上网。这是联网的时候，运营商的小哥这么设定的。我建议改成光猫使用桥接模式，并通过路由拨号的方式上网。

### 光猫设置

光猫的后台地址一般是http://192.168.1.1

光猫的后面有一个账号useradmin和密码，但是这个账号权限太少，无法设置网络相关的东西，我们这里需要用到超级管理员，移动的超管账号是: CMCCAdmin  密码：aDm8H%MdA

登录之后，查看 网络 =>宽带设置=> 连接名称，连接名称一般有很多下拉选项。

![移动宽带连接](https://cdn.yearnfar.com/blog/22/06/667884ce8dec1c271a33973557ea5398.png)

其中有一项是PPPoe拨号的，选中该连接名称。 

记录下宽带账号！密码如果没有修改过的话默认是手机号后六位，如果改过又不记得也可以去中国移动官网重置密码。

将模式改为Bridge、协议模式改为IPv4 & IPv6，并保存。其他的都可以不改（也不要去随便修改，我遇到过改了Vlan 设置导致拨号一直连不上网的情况，好在我在修改之前截图备份了，把参数改回去之后又正常了）。

![移动宽带联接2](https://cdn.yearnfar.com/blog/22/06/4496a17629f3097db436e063b62575fb.png)

保存之后，查看 状态 => 网络侧信息，可以看到有一条类型是Bridge，协议是 IPv4 & IPv6，状态是 Connected 的记录。到这里，光猫部分的设置就已经完成了。

![移动网络状态](https://cdn.yearnfar.com/blog/22/06/3e2aa5804ac1f2658119b79951db61e2.png)

### 路由设置

进入到路由的后台，选择宽带账号上网，输入宽带账号，宽带密码，并联接，即可上网。

![路由拨号上网](https://cdn.yearnfar.com/blog/22/06/8981316d1f4b44d91719dff73a15573f.png)

找到IPv6的设置菜单，打开IPv6，如果没有这个菜单，说明路由并不支持IPv6。

![路由ipv6开关](https://cdn.yearnfar.com/blog/22/06/c3c12c4d265a3bc5c456155834543c51.jpg)



### 验证IPv6

#### 方式一：

浏览器打开 http://test-ipv6.com/ 验证是否支持IPv6。

![ipv6测试结果](https://cdn.yearnfar.com/blog/22/06/3d1ce966ae9b3aaa977995c90e953f00.png)

#### 方式二

终端通过CURL获取当前电脑IPv6地址

```shell
➜  ~ curl https://myip6.ipip.net
当前 IP：2409:8a55:628:4250:4982:c8ad:****:****  来自于：中国 广东 广州  移动
```





